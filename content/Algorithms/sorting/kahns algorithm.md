---
tags: [algorithms, graphs, sorting, big-o]
related: "[[introduction to algorithms]]"
created: 2026-10-17
---

# Kahn's algorithm

Kahn's algorithm is a BFS based algorithm used for topological sorting. If the graph has an edge `u -> v` the algorithm should ensure that vertex `u` will always come before `v`. With this in mind we can conclude that Kahn's algorithm only works on directed graphs that have no cycles (DAGs). This is a topological sort which takes the following form:

```text
ALGORITHM TopologicalOrder(count, edges)
    // edges are (parent → child): parent must come before child

    // in-degree: how many parents each node still needs
    waiting_on ← [0] × count
    // adjacency list, parent → its children
    children   ← [[ ]] × count
    FOR EACH (parent, child) IN edges DO
        children[parent].PUSH(child)
        waiting_on[child] ← waiting_on[child] + 1

    // FIFO, seeded in id order
    ready ← [ n FOR n IN 0..count IF waiting_on[n] = 0 ]
    order ← [ ]

    WHILE ready ≠ ∅ DO
        node ← ready.POP_FRONT()
        order.PUSH(node)
        FOR EACH child IN children[node] DO
            waiting_on[child] ← waiting_on[child] − 1
            IF waiting_on[child] = 0 THEN
                ready.PUSH_BACK(child)

    IF |order| = count THEN
        RETURN Ok(order)
    ELSE
        // every node still blocked is in or downstream of a cycle
        RETURN Err([ n FOR n IN 0..count IF waiting_on[n] > 0 ])
```

---

## Connections

- The traversal underneath it is [[Breadth First Search]]: same FIFO worklist, but a node is pushed when its in-degree reaches zero rather than on first discovery, so the order comes out by dependency instead of by distance.
- Sits with [[introduction to algorithms]]. "Parent before child" is a well specified output in that note's sense, and the $|order| = count$ test at the end is the specification checking itself: a node is emitted only once its last parent has been, so a short answer means something was never unblocked. Note the ordering is not unique — seeding `ready` in id order is a tie-break, not part of the requirement.
- The sweep this replaces is in [[Bitset Auth]]. There the roles are numbered so that every parent has a lower index than its children, which makes $A$ strictly lower triangular and lets one pass in index order build the closure. That numbering _is_ a topological order, assumed rather than computed; Kahn's is what you run when the ids do not already come that way, and "no cycles" is the same precondition read from the other end.
- The in-degree array is counting a set: $\text{waiting\_on}[v] = |\{\,u : (u, v) \in E\,\}|$, and the edge set is a relation $E \subseteq V \times V$ — the same covering-relation formalism as $P \subseteq R \times R$ in [[Bitset Auth]], unpacked in [[sets]]. Transitive closure asks _who is reachable_; a topological sort asks _in what order_, from the same relation.
- The cost argument is the amortised counting of [[Monotonic Stack]]: every node is pushed onto `ready` at most once and every edge is decremented exactly once, so the total is $O(V + E)$ rather than the product of the two loop bounds. Count how many times each item can be consumed, not how deep the nesting goes.
- The recursive alternative is DFS with post-order emission, which is a recurrence with a base case in the sense of [[basic recursion for sum loop]] — same ordering, cycle detection by grey-node rather than by counting, and a call stack instead of a queue.
- The adjacency list is a matrix in disguise: $A_{uv} = 1$ when $(u, v) \in E$, and a digraph is acyclic exactly when its rows and columns can be permuted to make $A$ strictly triangular — which is the permutation this algorithm computes. Matrix form and Boolean products are in [[Matrix algebra]] and [[Linear Algebra]].
- Why a CAD kernel needs it: a parametric model is a dependency graph of features and constraints, so a rebuild has to evaluate parents before children and a cycle is a user error to report rather than a crash — hence the `Err` branch returning the still-blocked nodes. The geometry being rebuilt is [[CAD Geometry]], and the same ordering governs GPU work whose passes consume each other's resources.
- Part of [[Home]].

## Open

- [ ] A worked trace on a small DAG, queue state per step, including a cycle case to show what `Err` returns.
- [ ] The $O(V + E)$ cost written out with constants, as in [[Monotonic Stack]].
- [ ] Rust implementation, and whether `ready` wants a `VecDeque` or a `Vec` used as a stack (DFS order) — the choice changes which valid ordering comes out.
- [ ] Lexicographically smallest order with a min-heap instead of a FIFO, and what that costs.
