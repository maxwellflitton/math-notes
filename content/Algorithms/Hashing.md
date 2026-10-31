---
tags:
  - algorithms
  - skiena
  - hashing
  - data-structures
source: Skiena, The Algorithm Design Manual, §3.7
related: "[[introduction to algorithms]]"
written: 2026-10-31
---

# Hashing

## Hash functions

A hash function is essentially a function that converts a key into an integer. For the equation below we have a hash function for converting a string into a number:

$$
H(S) = \alpha^{|S|} + \sum_{i=0}^{|S|-1} \alpha^{\,|S|-(i+1)} \times \mathrm{char}(S_i)
$$

$S$ is the string, $\alpha$ is the total number of letters in the alphabet, and $\mathrm{char}$ is a function that converts a character to a number. $|S|$ is just the length of the string. For example, let's hash the string `"abc"`:

$$
H(abc) = 26^{3} + \left(26^{3-1} \times 0\right) + \left(26^{3-2} \times 1\right) + \left(26^{3-3} \times 2\right)
$$

where $|S| = 3$, $\alpha = 26$ and the char function is zero-based:

$$
a = 0,\quad b = 1,\quad c = 2
$$

We can simplify this to:

$$
H(abc) = 26^{3} + 0 + 26 + 2 = 17{,}604
$$

> [!note] Changed from the handwritten version
> The page used $a=1, b=2, c=3$, giving $18{,}307$. Per the "note to LLM" it has been shifted to zero-based ($a=0, b=1, c=2$), which gives $17{,}604$. Both are correct under their own char mapping; the index below has been recomputed to match.

> [!note] Check against Skiena
> Skiena's string hash is just the sum, $H(S) = \sum_{i=0}^{|S|-1} \alpha^{|S|-(i+1)}\,\mathrm{char}(S_i)$. The leading $\alpha^{|S|}$ term looks like an addition of yours. It's harmless (it's a constant for a given length, so it doesn't hurt distribution) but worth checking against the book.

## Reducing to a bucket index

We can see that $17{,}604$ is high just for `"abc"`. Larger strings will be even bigger. Therefore, we need to take the modulo. Let's say that we have a slice of 4 buckets. We can calculate the index of the key with the following:

$$
I_4\big(H(abc)\big) = 17{,}604 \bmod 4 = 0
$$

The modulo is the remainder, so the modulo of 4 can only be $0, 1, 2, 3$. If it ties over to 4 then there is no remainder, therefore it's zero again. (That's exactly what happened here: $17{,}604 = 4 \times 4{,}401$, so it lands on bucket 0.) The modulo is a great trick for indexing and queues. However, be careful: modulo is division and it is expensive.

> [!tip] Power-of-two trick
> If the number of buckets is a power of two, $m = 2^k$, then $h \bmod m$ is the same as $h \,\&\, (m-1)$, a single AND instruction instead of a division. This is why most hash table implementations size their bucket array as a power of two.

## Collisions

Because we don't have arrays with infinite size, hashes will collide when trying to allocate a bucket to a hash. One way of solving this is having chains off each bucket so we can add the colliding elements in the chain. However, this takes extra memory (a pointer per bucket and a node per element) that could instead have been used to make the table bigger and reduce the collisions.

**Open addressing** is where we probe by moving the pointer up one if there's a collision. This keeps everything in one contiguous array, which is good for caching. Deletion is done by tombstoning the slot rather than removing it (otherwise probe sequences would break), but then you have compaction as tombstones build up.

> [!note] Clarified
> The handwritten line "this makes the caching and deletion (if tombstoned)" was garbled; the above is the intended meaning: open addressing is cache-friendly, deletion needs tombstones, tombstones eventually force a rehash/compaction.

Chaining and probing both cost $O(m)$ to initialise, where $m$ is the number of buckets.

## Other applications of hashing

Hashing has other applications. The idea of a hash is to represent a large object such as a string by a large number. It is very unlikely that two different large objects will result in the same number.

- **Duplicate detection.** Hashing an entire webpage is a quick way of finding duplicate webpages.
- **Plagiarism detection.** If we wanted to detect plagiarism, we can have a hash table of overlapping windows of the document, and look for windows whose hashes appear in both.
- **Change detection.** Cryptographic hashing is also used to detect changes in network packets or files (integrity checks: if the hash differs, something changed).
- **Canonicalisation / fingerprinting.** We can also use hashing for canonicalisation, or compaction such as fingerprinting: a short fixed-size value that stands in for the big object.

## Connections

- Sits with [[introduction to algorithms]], and moves the goalposts of that note's correctness framing. A hash table is correct no matter how bad $H$ is: lookups compare keys for equality, so a terrible hash costs *time*, never a wrong answer. What the hash function buys is distribution, and the guarantee it gives is expected rather than worst case — $O(1)$ on average, $O(n)$ if everything lands in one bucket. That is a different kind of claim from "every input gives the right output", and adversarial input (HashDoS) is the case where the average stops being the thing to reason about.
- $H(S) = \sum_{i} \alpha^{|S| - (i+1)}\,\mathrm{char}(S_i)$ *is* positional notation, with the alphabet size as the base: the string is read as a number in base $\alpha$ exactly as [[Binary Numbers]] reads a bit pattern in base 2. The implementation is the same recurrence, $v \leftarrow \alpha v + \mathrm{char}(S_i)$ against that note's $v \leftarrow 2v + b$ — Horner's rule, a recurrence with an accumulator in the sense of [[basic recursion for sum loop]]. Index laws for $\alpha^k$: [[exp-algebria-rules]].
- The power-of-two trick is the $2^k - 1$ low mask: $h \bmod 2^k = h \,\&\, (2^k - 1)$, which is place value from [[Binary Numbers]] and `low_mask` in [[Bit Shifting operations]]. The same identity is why a [[Ring Buffer]] sized to a power of two wraps with `& (n - 1)` instead of `%`, and the reason both data structures prefer those capacities is this one line of arithmetic.
- The exact opposite of [[Bit Encoding]], and the pair is worth holding together. There the map is injective — `pack_id` and its decode are inverse functions and the test asserts the round trip, so a collision would be a bug. Here the map is many-to-one *by construction*: the point is to compact an unbounded key into a fixed-width integer, so collisions are a structural certainty to be handled rather than a defect to be fixed. Both squeeze a large object into one number; only one of them can be undone.
- Chaining is a [[Linked Lists|linked list]] per bucket, and the memory objection in this note is that note's trade-off restated: a pointer per bucket and a node per element buys you unbounded occupancy and costs you cache locality. Open addressing is the contiguous answer, which is why it probes — and the arena-of-indices alternative to `Box` there is the same instinct as keeping everything in one array here.
- A hash table is set membership made cheap, in the sense of [[sets]]: duplicate detection is testing $x \in S$, and the plagiarism case is an intersection of two sets of window hashes. [[Bitset Auth]] is the other representation of the same operation — one bit per element, exact and collision-free, but it needs a small dense universe of keys. Hashing is what you reach for when the universe is huge and sparse, and the price of that is approximation in the bucket index.
- Where `visited` comes from in [[Breadth First Search]]. For a graph with dense integer ids that set is an array or a bitmask; for the implicit graphs that note describes — mesh triangles keyed by an index triple — there is no natural slot, so membership is a hash set and the traversal's $O(V + E)$ rests on the expected $O(1)$ above.
- Rehashing is amortised doubling, the counting argument of [[Monotonic Stack]] in its other classic form: each grow moves every element once, but the sizes form a geometric series $n + \frac{n}{2} + \frac{n}{4} + \cdots < 2n$, so $n$ inserts cost $O(n)$ total even though one of them is $O(n)$ on its own. The geometric sum itself is the $\sum_{k} 2^k = 2^{n} - 1$ of [[Binary Numbers]].
- At system scale this becomes a partitioning decision: sharding by hash of key spreads load evenly but destroys range scans, and every hash index is derived data rebuildable from the system of record — [[Trade-offs in Data Systems Architecture]].
- Part of [[Home]].

## Open

- [ ] [[Hash map collision handling]] — open addressing against chaining written out: load factor, linear versus quadratic probing, Robin Hood, and what hashbrown/SwissTable does with its SIMD metadata bytes.
- [ ] The expected-cost analysis with load factor $\alpha$: chains as $1 + \alpha$ and probes as $\frac{1}{1 - \alpha}$, plus the amortised doubling argument written with constants as in [[Monotonic Stack]].
- [ ] Settle the leading $\alpha^{|S|}$ term against Skiena (see the callout above) and write the Horner form that an implementation would actually use.
- [ ] Rolling hash (Rabin–Karp) for the overlapping-window case — the whole point is getting the next window's hash in $O(1)$ from the previous one, which the formula above does not give as written.
- [ ] Cryptographic against non-cryptographic hashing: collision resistance, and why Rust's default hasher is SipHash rather than something faster.
- [ ] Rust implementation of both collision strategies over the same `Hasher` trait, with a distribution test on real strings.
