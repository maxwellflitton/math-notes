---
tags:
  - reading
  - ddia
  - data-systems
  - reliability
book: DDIA 2nd ed.
chapter: 2
author: Kleppmann & Riccomini
source: DDIA 2nd ed., ch. 2 — Reliability and Fault Tolerance
related: "[[Trade-offs in Data Systems Architecture]]"
written: 2026-11-15
---

# Reliability and Fault Tolerance

Reading notes on the *Reliability and Fault Tolerance* section of chapter 2 of *Designing Data-Intensive Applications*, 2nd ed. Follows on from [[Describing Performance]]. Part of [[Home]].

## Faults versus failures

First we need to clarify terms:

- **Fault**: a particular part of a system stops working, like a hard drive or a single machine crashing.
- **Failure**: the system as a whole fails, meaning it stops providing the service the user needs.

Which of the two a given event is depends on where you draw the boundary. A crashed disk is a *failure* from the disk's point of view, but only a *fault* from the point of view of a system that can survive losing it. **Fault tolerance** means stopping faults from turning into failures.

## Fault tolerance

A good way to see the distinction is hard drives. If the entire system is on one hard drive and it fails, we have a system failure. However, if we have a system on 10 hard drives and one fails, we have a fault. This leads us naturally to fault tolerance.

The 10 drives only turn the failure into a fault if the data is **redundant**: replicated or in RAID, so another drive holds a copy. Ten drives with the data split across them and no copies is *worse*, because now any one of the ten can take the system down. A component whose loss alone brings the whole system down is a **single point of failure (SPOF)**.

This may seem excessive, but remember 1–2% of hard drives fail every year, and this can happen to new hard drives. As your system gets bigger, faults become more common.

Published failure rates vary by drive type and vendor. Magnetic disks are commonly quoted at a few percent a year, SSDs lower. New drives failing is real: early failures followed by a long quiet period and then wear-out is called the **bathtub curve**.

With an annual failure rate $r$ per drive and $N$ independent drives, the expected number of failures in a year is $N r$, and the chance that at least one fails is

$$
P(\text{at least one fault}) = 1 - (1 - r)^N
$$

With $r = 0.02$ and $N = 10$: $1 - 0.98^{10} \approx 0.18$. With $N = 10{,}000$ you expect about 200 a year, so roughly one every couple of days. At that scale disk faults are routine maintenance, not incidents. This is the same formula as tail latency amplification in [[Describing Performance]]: a small chance per part becomes near-certainty across many parts.

We also need to take into account patches, updates, etc. with tolerance. If everything is running on one server, then updates can be disruptive.

A system that can tolerate losing a whole machine can be updated with a **rolling upgrade**: restart one node at a time onto the new version while the others keep serving, so there's no planned downtime. Fault tolerance therefore helps with planned maintenance as well as accidents.

On the flip side, software faults are more risky than hardware faults, so the more nodes that run your software, the more faults you will likely get.

The reason software faults are riskier is that they are **correlated**. Hardware faults are mostly independent: one disk dying says little about the next. A software bug, though, is on *every* node running that code, and the same input or date triggers it on all of them at once. So adding nodes protects you against hardware faults but not against software faults — ten replicas of buggy code fail together. The leap-second bugs (2012) and the CrowdStrike update (2024) that took down millions of Windows machines at the same moment are both examples.

We also must consider cascading failures and new behaviours when two independent systems talk to each other. A classic example is a circular dependency: a compiler can detect this, but two services can get caught in a loop requesting from each other.

Two failure modes worth naming here:

- **Cascading failure**: one overloaded or failed component pushes its load or errors onto others, which then fail too. The retry storms in [[Describing Performance]] are one way this spreads.
- **Circular dependencies at startup**: if A needs B to boot and B needs A, then after a full outage neither can come back up. A compiler catches the cycle because it has the whole dependency graph up front (a topological sort fails, see [[kahns algorithm]]). Services only discover their dependencies at runtime, so nothing checks the graph for them.

## Humans and outages

When it comes to outages, human errors and configuration issues were the biggest cause. However, it is too simplistic to blame the human. Also, we must be careful of solutions that are too simplistic in general.

The book's view is that a "human error" is usually a symptom of a system that made the error easy and its effects large. Instead of blaming the person:

- **Blameless postmortems**: people report honestly what happened, so the real causes come out.
- Make the right thing easy: good interfaces and APIs, sandboxes to try things safely.
- Limit the blast radius: gradual rollouts, quick rollback, feature flags.
- Detect problems early: monitoring and telemetry.

## Connections

- Follows [[Trade-offs in Data Systems Architecture]], which listed fault tolerance as the first reason to distribute and partial failure as the price; this section is about paying that price on purpose. That note also answers the boundary question above directly: losing derived data — a cache, an index, a warehouse table — is a *fault*, because "where would I rebuild this from" has an answer, while losing the system of record is a *failure*. Redundancy is derived data created deliberately for exactly that reason.
- The other half of the chapter, [[Describing Performance]], and the same piece of arithmetic: $1 - (1-p)^N$ across a fan-out of calls is $1 - (1-r)^N$ across a shelf of drives. Backoff and circuit breakers belong to both notes, and a retry storm is a cascading failure measured in latency rather than in errors — nothing has failed, every part is inside its budget, and the system still cannot recover on its own.
- Correlated against independent is also the line between reliability and correctness, in the sense of [[introduction to algorithms]]. A bug is deterministic: it is a counterexample to a specification, so it fires on every replica that meets the same input, at the same moment. Hardware faults are probabilistic and roughly independent, which is the only reason redundancy helps at all. Adding nodes multiplies the dice rolls but merely copies the proof — so it buys protection against the first kind of fault and none at all against the second.
- The arithmetic behind $1 - (1-r)^N$ is complement and intersection in the notation of [[sets]]: "at least one fault" is the complement of "all $N$ survive", and that intersection only factorises into $(1-r)^N$ when the events are independent. The correlated software fault above is precisely where the factorisation is invalid, and the product formula then overstates the protection badly — which is why the number is worth deriving rather than quoting.
- A constant failure rate is an ODE in disguise — [[First order differential equations]]. Drives dying at a rate proportional to how many are left gives $\frac{dS}{dt} = -\lambda S$ and $S(t) = S_0 e^{-\lambda t}$, which is *memoryless*: age carries no information, and an annual rate is $r = 1 - e^{-\lambda}$. The **bathtub curve** is the statement that this model is wrong at both ends — $\lambda$ is high for new drives, flat in the middle, rising with wear — so the 1–2% a year above is an average over a curve rather than a constant.
- $(1-r)^N = e^{N\ln(1-r)} \approx e^{-rN}$ for small $r$, by the log laws of [[exp-algebria-rules]]. That is why the expected count $Nr$ and the chance of at least one fault agree while $Nr$ is small and part company once it is not: at $N = 10$ both are about $0.2$, while at $N = 10{,}000$ the expectation is 200 failures and the probability of at least one is indistinguishable from certainty.
- The circular-dependency case is a missing topological sort — [[kahns algorithm]]. A compiler holds the whole dependency graph up front, so the sort fails and names the cycle; services discover their edges only when they call each other, so nothing ever runs the check. Boot order after a full outage *is* a topological order, and a cold start is the one moment you need it.
- [[Bitset Auth]] is the acyclic case built in by construction: roles are numbered so that every parent has a lower index, which makes the closure one sweep and makes a cycle unrepresentable rather than merely detected. It is also the blast-radius argument at the scale of a single line — the OR-against-addition mistake there is a privilege escalation, caught by a test instead of by an outage.
- Cycle tolerance at runtime rather than at compile time: the `visited` set of [[Breadth First Search]] is what stops a loop in the graph turning a traversal into a non-terminating one. The service equivalent is a hop count or a deadline carried along with the request, so that a request loop between two services dies rather than circulating.
- "Make the right thing easy" is a type-level claim, and the vault's sentinel notes are the small version of it. `None` rather than `i32::MIN` in [[Binary Tree Validation]], and the all-zero id meaning "nothing" in [[Bit Encoding]], make the dangerous state unrepresentable instead of merely documented — the same move as a sandbox or a staged rollout, one level down. A sentinel chosen from inside the key domain is a human error waiting for the right input.
- Part of [[Home]].

## Open

- [ ] Explain this and [[Describing Performance]] back together, Feynman-style, and get quizzed.
- [ ] Correlated faults in my own stack: what single bug or bad config would hit every node of the CAD app at once?
- [ ] MTTF, MTBF and availability as nines: components in series multiply their availabilities, $\prod_i a_i$, while parallel redundancy multiplies the *un*availabilities, $1 - \prod_i (1 - a_i)$ — the same complement argument as above, applied to uptime instead of to drives.
- [ ] RAID levels and erasure coding, where the question is $k$-of-$n$ rather than one-of-$N$: "enough shards survive" is a binomial tail, $\sum_{j=k}^{n} \binom{n}{j} (1-r)^{j} r^{\,n-j}$, not the single-event formula in this note.
- [ ] Where independence fails in hardware too: a shared power rail, rack, switch or cooling loop, and a batch of drives from one factory. "Independent" has to mean more than "a different machine".
- [ ] Byzantine faults, and the crash-stop against crash-recovery against arbitrary-behaviour split, once Ch9 reaches them.
- [ ] Chaos engineering (deliberately injecting faults to check tolerance actually works): worth a note when the book returns to it.
