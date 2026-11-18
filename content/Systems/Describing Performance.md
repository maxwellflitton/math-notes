---
tags:
  - reading
  - ddia
  - data-systems
  - performance
book: DDIA 2nd ed.
chapter: 2
author: Kleppmann & Riccomini
source: DDIA 2nd ed., ch. 2 — Describing Performance
related: "[[Trade-offs in Data Systems Architecture]]"
written: 2026-11-15
---

# Describing Performance

Reading notes on the *Describing Performance* section of chapter 2 of *Designing Data-Intensive Applications*, 2nd ed. Follows on from [[Trade-offs in Data Systems Architecture]], and pairs with [[reliability and fault tolerance]] from the same chapter. Part of [[Home]].

## Overload and why we need metrics

When a system is failing due to overload, it can get into a state that is impossible to get out of on its own: it is so busy that requests time out, clients retry, and the retries add even more load. So **exponential backoff** (with random jitter, so the clients don't all retry at the same moment) and **circuit breakers** (stop calling a service that keeps failing, try again later) can help regain a stable state.

Self-sustaining overload of this kind is a **metastable failure**, or a **retry storm**. Two more tools belong to the same family:

- **Load shedding**: the server rejects some requests early and cheaply so the rest can still be served.
- **Backpressure**: the server tells clients to slow down instead of buffering without limit.

We also need some clarification with metrics:

- **Response time**: what the client sees, so this includes all the delays in the system.
- **Service time**: the time it takes for the service to actually process a request.
- **Queueing delays**: time spent waiting before work can continue. This can be waiting for a CPU, or for a network packet sitting in a queue (in a switch or the OS buffer) to be sent or received.
- **Latency**: a catch-all term for time when the request isn't being processed: it is *latent*, waiting for service.

Roughly:

$$
\text{response time} = \text{service time} + \text{queueing delays} + \text{network latency}
$$

In everyday speech people use "latency" to mean response time. The book keeps them separate, so it's worth being precise when reading.

Queueing is also why a few slow requests hurt the fast ones: a server can only handle a small number of things in parallel, so one slow request holds up everything queued behind it (**head-of-line blocking**). That is why response time should be measured on the client side.

## Statistics for network stats

When it comes to network stats we can use the following:

- **Mean**: useful for estimating throughput. However, it's not good if you want to know a *typical* response time, because it doesn't tell you how many users actually experienced that delay. A few very slow requests drag the mean up even if almost everyone was fast.

- **Percentiles**: if we have a 95th percentile that is less than 1.5 seconds, then we can say only 5 out of 100 requests take more than 1.5 seconds. However, the 5% in our example (the **tail latency**) can tell us interesting things about the system, such as a user with a large data history having these slow responses. These percentiles can essentially be the canary in the coal mine.

The definition: sort all the response times in a window, and the $p$-th percentile is the value $t_p$ such that $p\%$ of requests are at or below it:

$$
P(T \le t_p) = \frac{p}{100}
$$

The **median** is $t_{50}$ (p50): half of requests are faster, half slower. Tail percentiles are usually p95, p99 and p999.

The book's example is Amazon, which tracks p999 for internal services. The customers with the slowest requests are often the ones with the most data on their accounts, which tends to mean they are the most valuable customers. Past a point, chasing very high percentiles costs more than it's worth, because random events you can't control dominate.

## Statistics can be misleading

Sometimes statistics can be misleading. There was one study that showed that faster response times led to *less* conversion. It turned out that error response pages load very fast, so the fast group was full of failed requests. There's also quality of response: a slower but better search can increase conversion.

> [!tip] Correlation is not causation
> The error-page case is a **confounder**: a hidden variable (the request failed) that caused both the fast response and the lost sale. Always ask *what* was fast before deciding that speed was the cause.

Percentiles also tell us the weakest link in a chain of requests. If one user request calls several backends in parallel, it is only as fast as the slowest of them. This is **tail latency amplification**: if each backend call is slow with probability $p$, and a request fans out to $N$ independent calls, the chance that the user request is slow is

$$
P(\text{slow}) = 1 - (1 - p)^N
$$

With $p = 0.01$ (a slow p99) and $N = 100$, that gives $1 - 0.99^{100} \approx 0.63$. So 63% of user requests hit at least one slow call, even though each backend is "fast 99% of the time". This is why the tail matters more as systems fan out.

## Computing percentiles

For computing percentiles, it's usual to keep a sliding window of the last 10 minutes of response times, sorted, and read the percentiles off that.

Sorting the whole window every time is the naive version and gets expensive. In practice people use approximate algorithms that keep a compact summary: **HdrHistogram**, **t-digest**, **DDSketch** or OpenTelemetry's histograms.

A trap worth remembering: **you can't average percentiles**. The mean of each machine's p99 is not the p99 of the whole system. To combine them, add the histograms together and then read the percentile off the combined histogram.

## Connections

- Follows [[Trade-offs in Data Systems Architecture]], which named this as the Ch2 topic ("percentiles rather than averages") and listed partial failure as the price of distributing. Tail amplification is that price denominated in milliseconds: nothing has failed, every backend is inside its SLO, and the user request is still slow. The same note's warning that cost is a vector rather than a single number is why one number — the mean — cannot describe a response time either.
- The other half of the same chapter, [[reliability and fault tolerance]], and the same piece of arithmetic. $1 - (1 - r)^N$ over $N$ disks is $1 - (1 - p)^N$ over $N$ backend calls: independent events, a small probability per part, near-certainty across many parts. Backoff and circuit breakers belong to both notes, and a retry storm is a cascading failure seen from the latency side rather than the error side.
- The measured version of the average-case-against-worst-case split in [[introduction to algorithms]]. The mean and the p99 are two statistics of the same distribution in the way that average and worst case are two claims about the same algorithm — and big-O is silent about both: it drops the constants and says nothing about queueing, so an $O(1)$ lookup can still be a two-second p99. The percentile is what measures the gap between the asymptotic claim and the clock.
- [[Monotonic Stack]] is the counterpart argument. Amortised analysis says the *total* over $n$ operations is bounded even when one operation is expensive, which is a reason not to worry about the single worst case. A tail latency is the opposite case: the total is fine and the individual request is not. The same averaging that makes an amortised bound honest makes a response-time target dishonest, because the user waiting on the expensive operation is the one being averaged away.
- The 10-minute sliding window is a [[Ring Buffer]]: fixed capacity, the oldest sample overwritten by the newest, nothing allocated per sample. That bounded capacity is also the load-shedding answer to buffering without limit — drop the old rather than grow — which is the same choice the ring makes on every `push` when it is full.
- Head-of-line blocking is FIFO discipline, and the queue in front of a busy server is the structure of [[Linked Lists]]: the cost of one slow request is paid by everything linked behind it, because position in the queue, not urgency, decides who is served next. Shedding and priority queueing are both ways of breaking that ordering on purpose.
- A percentile is an **order statistic**: $t_p$ is the element of rank $\lceil pn/100 \rceil$ in the sorted window, so reading a percentile is a rank query rather than an aggregate. That is where this meets [[Binary Tree Validation]] — the in-order sweep of that note *is* the sorted window, and a search tree carrying subtree counts answers a rank query in $O(h)$ without sorting anything.
- HdrHistogram's buckets are read straight off the bit pattern, in the sense of [[Binary Numbers]]: the position of the highest set bit chooses the bucket (an exponent) and the next few bits choose the sub-bucket (a mantissa), so a value's bucket index is `extract(value, shift, width)` from [[Bit Shifting operations]] rather than a division. Constant *relative* precision comes out for free, which is exactly what a percentile needs — resolution where the values are, not a fixed grid.
- [[Hashing]] does the same squashing of an unbounded value into a small index, with the opposite requirement. A hash function wants to destroy order and spread keys evenly; a histogram bucket must *preserve* order, because the percentile is read by walking buckets in value order and accumulating counts. DDSketch does it with $i = \lceil \log_\gamma x \rceil$: logarithmic buckets, so the relative error is fixed and the index is still monotone in $x$.
- The algebra in both formulas above is [[exp-algebria-rules]]. $(1 - p)^N$ is the index law doing the work in the fan-out, and $\ln(1 - p) \approx -p$ for small $p$ turns it into $1 - e^{-pN}$ — with $pN = 1$ that is $1 - e^{-1} \approx 0.63$, the number computed above. The log-scaled buckets are the same laws read the other way: $\log_\gamma x$ converts a multiplicative bucket width into an additive index.
- A retry storm is positive feedback, and it is the birth-rate model of [[First order differential equations]] with the sign flipped the wrong way. Retries are births and completions are deaths, so $\frac{dL}{dt} = (b - c)L$: while retries outpace completions the load grows like $e^{rt}$ and the system cannot recover on its own. Exponential backoff is the damping term — it makes the retry rate decay like $e^{-\Gamma t}$, the envelope of [[dampened harmonic oscillator]] — and "metastable" is that note's stability question exactly: an equilibrium that holds until something pushes it far enough, after which the exponent has the wrong sign.
- Why the averaging trap is a trap, in the language of [[Basic Operations]]. A histogram is a vector of bucket counts, and merging machines is vector addition: componentwise, associative, order-independent, so the combined histogram is exact. The quantile read off it is a *nonlinear* function of that vector, and nonlinear functions do not commute with averaging — $\operatorname{mean}_i\big(t_{99}(h_i)\big) \ne t_{99}\big(\sum_i h_i\big)$. It is the same reason the mean is a poor summary within one machine: addition is linear, and the tail is not.
- Part of [[Home]].

## Open

- [ ] Explain this section back and get quizzed on it, Feynman-style.
- [ ] Coordinated omission: when the load generator waits for a slow response before sending the next request, the slow periods are under-sampled and the measured tail looks better than it really is.
- [ ] Little's law, $L = \lambda W$, and why response time goes vertical rather than degrading linearly near saturation: mean response time $\frac{1}{\mu - \lambda}$ for an M/M/1 queue, which is $\frac{1/\mu}{1 - \rho}$ in terms of utilisation $\rho = \lambda/\mu$. The $\frac{1}{1 - \rho}$ factor is the same shape as the probe count $\frac{1}{1 - \alpha}$ against load factor in [[Hashing]] — both are a queue filling up.
- [ ] The expected maximum of $N$ samples, as the quantity the fan-out formula is really about: why adding backends punishes the tail even when no single backend gets slower.
- [ ] HdrHistogram's bucket layout written out in Rust, with `extract` from [[Bit Shifting operations]] picking exponent and mantissa, plus a check of the relative-error bound.
- [ ] The GPU analogue of head-of-line blocking: a workgroup in [[Shaders]] finishes when its slowest invocation does, so one divergent branch is paid for by every lane in lockstep with it.
- [ ] Which percentile matters in the CAD app? Probably sync round-trip at p99 for collaborators on large documents.
