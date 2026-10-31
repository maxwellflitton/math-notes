---
tags:
  - reading
  - ddia
  - data-systems
  - architecture
book: DDIA 2nd ed.
chapter: 1
author: Kleppmann & Riccomini
source: DDIA 2nd ed., ch. 1
written: 2026-10-31
---

# Trade-offs in Data Systems Architecture

Reading notes on chapter 1 of *Designing Data-Intensive Applications*, 2nd ed. (Kleppmann & Riccomini). Part of [[Home]].

> [!warning] Provenance
> This note was reconstructed by Claude from the chapter's general concepts, not from Maxwell's own notes (the originals were never written up in chat or the vault). Treat it as a scaffold: explain each section back in your own words and replace the text, Feynman-style.

The chapter's thesis: there is no single "right" data architecture. Every choice is a trade-off, and the job is to understand the axes so the trade-off is made deliberately rather than by default.

## Operational versus analytical systems

**Transaction processing (OLTP)** serves end users. Each request touches a small number of records, looked up by key, and the system must answer quickly because someone is waiting. The data is the live state of the business: current orders, current balances, current documents.

**Analytics (OLAP)** serves analysts and decision-makers. A query scans a large number of records and aggregates them (counts, sums, averages over a period). Latency of seconds or minutes is acceptable; throughput over large scans is what matters.

| | Operational (OLTP) | Analytical (OLAP) |
|---|---|---|
| Typical read | few rows by key | aggregate over many rows |
| Typical write | small, low-latency, random | bulk import or event stream |
| Users | application / end users | analysts, dashboards, ML |
| Data represents | current state | history of events over time |

The two workloads pull storage design in opposite directions (row-oriented vs column-oriented, which Ch4 covers), which is why they are usually separate systems.

### Data warehousing

Running analytics directly against production databases hurts the users who are waiting on them, so analytical data is copied out into a **data warehouse**. The pipeline is **ETL** (extract from the operational systems, transform into an analysis-friendly schema, load) or **ELT** (load raw first, transform inside the warehouse). Related terms: a **data lake** is a dump of raw files (often in an object store) with schema applied on read; a **lakehouse** tries to put warehouse-style query engines over lake-style storage.

### Systems of record and derived data

A **system of record** (source of truth) holds the authoritative copy of a fact. Writes go there first, and if another copy disagrees, the system of record wins.

**Derived data** is anything computed from the system of record: caches, secondary indexes, materialised views, search indexes, warehouse tables, ML features. It is redundant by definition and can be thrown away and rebuilt from the source.

> [!tip] The distinction is about role, not technology
> The same database can be a system of record for one dataset and hold derived data for another. The question to ask of any store is "if this were lost, where would I regenerate it from?" If the answer is "nowhere", it is a system of record.

## Cloud versus self-hosting

**Cloud services** trade money and control for reduced operational burden and elasticity. Pros: no hardware procurement, capacity on demand, managed upgrades, breadth of services. Cons: cost at steady high utilisation, vendor lock-in, egress fees, opaque failures you cannot debug, and the provider's roadmap becoming your roadmap.

**Self-hosting** is the reverse: you own the failure modes and the pager, but also the cost curve and the ability to change anything.

### Cloud-native system architecture

Systems designed for the cloud, rather than lifted into it, tend to share a shape:

- **Separation of storage and compute.** Durable data lives in object storage (S3-style); compute nodes are stateless and disposable. Scale either independently.
- **Multitenancy.** One deployment serves many customers, with isolation enforced in software rather than by separate machines.
- **Serverless / managed services.** You supply code or queries; the provider owns provisioning, scaling and most of the operations.

### Operations in the cloud era

The ops role does not disappear; it moves up a level. Less racking servers and patching kernels, more choosing, configuring, integrating and monitoring managed services, and owning reliability end-to-end (the SRE framing). Understanding how the managed services fail is still your problem.

## Distributed versus single-node systems

A single machine is simpler, faster for a given workload, and has no partial failures. Modern hardware is large; many systems that are distributed by reflex would fit on one well-specified node.

Reasons that justify distributing:

- **Fault tolerance.** One machine is a single point of failure; several machines in several locations can survive outages.
- **Latency and geography.** Put data near users, or satisfy data-residency law.
- **Scalability.** The workload genuinely exceeds one machine's CPU, memory, disk or bandwidth.
- **Elasticity.** Load varies enough that adding and removing machines is worth the complexity.

### Problems with distributed systems

The cost is a new class of failure: **partial failure**. Some nodes work, some do not, and you often cannot tell which. Networks lose, delay and reorder messages; clocks disagree; a node that looks dead may simply be slow. All of Ch9 is about this.

### Microservices and serverless

Splitting a system into services is primarily an **organisational** choice: teams can deploy independently and own their own data. The engineering price is that in-process calls become network calls, with all of the partial-failure problems above, plus harder debugging and versioning across service boundaries. Serverless functions push this further by making the unit of deployment a single function with no long-lived process.

### Cloud computing versus supercomputing

Both run on thousands of machines, but with opposite failure philosophies. **HPC** assumes a failure stops the whole job, so it checkpoints and restarts. **Cloud / internet services** assume something is always failing, so they design for continuous partial failure and keep serving. The second model is the one this book is about.

## Data systems, law and society

Data is not only an asset but a **liability**. Regulation (GDPR and successors) sets rules on what may be collected, how long it may be kept, and the right to access and erasure. Systems that cannot locate or delete an individual's data are a legal problem, not merely a design smell. The chapter frames this as a design input alongside performance and cost, not an afterthought.

## Summary

- Operational and analytical workloads differ enough to deserve different systems; derived data (warehouses, indexes, caches) is rebuilt from systems of record.
- Cloud buys operational relief and elasticity at the cost of money, lock-in and visibility; cloud-native design separates storage from compute.
- Distribute only for a concrete reason (faults, geography, scale, elasticity); the price is partial failure.
- Legal and ethical constraints on data are architectural requirements.

## Where this leads

Ch2 picks up the quality attributes of a single system: how to describe performance (percentiles rather than averages), reliability and fault tolerance, scalability, and maintainability.

## Connections

- The counterpoint to [[introduction to algorithms]]. That note's method is correctness against a specification, where one counterexample sinks an algorithm — and it names a search engine result as something *not* well specified. Architecture is that case everywhere: "which data system" has no provable answer, so the work is naming the axes (latency against throughput, cloud against self-hosting, one node against many) and choosing on purpose. Big O cannot settle a question whose objective is undefined.
- Systems of record and derived data are already in the vault in miniature. [[Bitset Auth]] computes the closure masks at startup from the role table and the covering relation: the table is the source of truth, the masks are a materialised view, and losing them costs a restart rather than a fact. [[Bit Encoding]] is the same split per frame — the scene is authoritative, the id texture the fragment shader writes is derived and regenerated every draw, which is why a packed id can be reused the moment the scene changes.
- Rebuilding derived data is a dependency problem, and the order is a topological sort: [[kahns algorithm]]. An ETL or ELT pipeline is a DAG of derivations where every parent must be refreshed before its children, a cycle is a modelling error to report rather than a crash, and the lower-triangular role numbering of [[Bitset Auth]] is that order assumed instead of computed.
- "Cost is a vector, not a single number" is the thesis of this chapter at machine scale, and [[Linked Lists]] argues it on one data structure: two $O(n)$ rotations differing in allocations and passes, not in order of growth. [[Ring Buffer]] is the bounded-capacity version of the same bargain — a fixed allocation that overwrites rather than growing, which is the backpressure question (drop, block or buffer) a page before it becomes an architectural one.
- Percentiles over averages, the Ch2 topic this chapter leads into, is the amortised argument of [[Monotonic Stack]] read from the other end. There a worst-case inner loop is harmless because the *total* is bounded; a tail latency is the case where the total is fine and the individual request is not, so averaging hides exactly the users who are suffering.
- Separation of storage from compute is the shape the GPU notes already assume: durable buffers uploaded once and bound by [[Bindings]], against disposable per-invocation work in [[Shaders]] that holds no state between frames. Shared-nothing parallelism, the same reason both scale by adding units.
- The vault itself is an instance: markdown files in git are the system of record, and every search index, backlink table and published Quartz page is derived data that can be deleted and rebuilt from them.
- Part of [[Home]].

## Open

- [ ] Replace each section above in my own words and delete the provenance warning — the scaffold is a reading aid, not a note yet.
- [ ] Ch2: latency against response time, and percentiles with the actual definitions (tail latency, coordinated omission), alongside the counting arguments in [[Monotonic Stack]].
- [ ] Ch4: column-oriented storage, as the layout answer to the OLTP/OLAP split in this note — the row-versus-column choice is a packing question in the sense of [[Bit Encoding]].
- [ ] Ch7: sharding by key range against sharding by hash of key, once [[Hashing]] is written up.
- [ ] A worked example from my own work rather than the book's: which store in the CAD stack is the system of record, and what is rebuildable from it.
