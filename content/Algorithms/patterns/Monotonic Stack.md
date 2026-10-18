---
tags: [algorithms, data-structures, stack, big-o]
source: Skiena, The Algorithm Design Manual (ch. 1–2); daily temperatures exercise
related: "[[introduction to algorithms]]"
created: 2026-10-03
---

# Monotonic Stack

A monotonic stack is a stack that uses greedy logic to keep its order. Greedy basically means one chess move ahead and nothing more: every decision is made on local data right now, with no lookahead and no backtracking. It is safe for this approach only if a popped element can never matter again.

We can have the following three types (bottom to top):

- same: $x[i] = x[i+1]$
- increasing: $x[i] < x[i+1]$
- decreasing: $x[i] > x[i+1]$

## Decreasing example

We can see how useful such a stack is with a decreasing example. We have an input of integers such as `[2, 1, 2, 4, 3]` and we want to calculate how many steps till a bigger number. We can do it with the following code:

```
Input:  nums, an array of n numbers
Output: steps, an array of n integers;
        steps[i] = distance to the next greater element, or −1 if none

steps ← array of n copies of −1
St    ← empty stack                 // holds indices; nums[St] decreasing bottom→top

for i ← n − 1 downto 0 do
    while St ≠ ∅ and nums[St.top] ≤ nums[i] do
        St.pop()                    // shadowed by nums[i], never needed again
    if St ≠ ∅ then
        steps[i] ← St.top − i
    St.push(i)
return steps
```

On `[2, 1, 2, 4, 3]` this gives `[3, 1, 1, −1, −1]`.

Invariant: the stack holds indices whose values are strictly decreasing from bottom to top. Anything smaller than or equal to the incoming element gets popped because the incoming element would always beat it as "next greater" for anything further left.

Flip `≤` to `≥` and you get an increasing stack, which solves next-smaller-element instead.

## Big O

We can calculate the Big O notation with the following constants:

- $c_1$ = cost of writing $-1$ to one array slot (done $n$ times)
- $c_2$ = creating an empty stack
- $c_3$ = one `St.pop()`
- $c_4$ = the test `St ≠ ∅`
- $c_5$ = `steps[i] ← St.top − i` (peek, subtraction and array write)
- $c_6$ = `St.push(i)` (one append)
- $c_7$ = returning the array pointer (not $n$-dependent as it is a pointer)

Let $p_i$ be the number of pops in iteration $i$. Our cost per iteration can be calculated with the following:

$$
\text{cost}(i) = c_3\,p_i + c_4 + c_5 + c_6
$$

The total can be calculated with the following:

$$
T(n) = c_1 n + c_2 + c_7 + \sum_{i=0}^{n-1}\bigl(c_3\,p_i + c_4 + c_5 + c_6\bigr)
$$

$$
T(n) = c_1 n + c_2 + c_7 + n\,(c_4 + c_5 + c_6) + c_3 \sum_{i=0}^{n-1} p_i
$$

Every pop removes something that was previously pushed, and nothing is pushed twice, so:

$$
\sum_{i=0}^{n-1} p_i \;\le\; (\text{total pushes}) = n
$$

So:

$$
T(n) \;\le\; c_1 n + (c_4 + c_5 + c_6)\,n + c_3 n + c_2 + c_7
$$

Here we can only see constants scaled by $n$, so our loop is $O(n)$. Since the outer loop alone does $n$ pushes it is also $\Omega(n)$, hence $\Theta(n)$.

Space: `steps` holds $n$ entries and the stack holds at most $n$ indices, so $O(n)$.

## Rust Code

We can solve a problem such as the temperature problem with the following code:

```rust
pub fn daily_temperatures(temperatures: Vec<i32>) -> Vec<i32> {
    let n = temperatures.len();
    let mut stack: Vec<usize> = Vec::new();
    let mut result = vec![0; n];

    // loop through the input back to front
    for i in (0..n).rev() {

        // drain the stack until the top of the stack is bigger
        // than the current value
        while let Some(&top) = stack.last() {
            if temperatures[top] > temperatures[i] {
                break;
            }
            stack.pop();
        }

        // insert the top stack into the 
        if let Some(&top) = stack.last() {
            // the top - i is the distance
            result[i] = (top - i) as i32;
        }
        stack.push(i);
    }
    result
}
```

### The general rule

The nested `while` looks quadratic but isn't. Whenever an inner loop *consumes* something an outer loop *produces* at a bounded rate (stack pops, queue dequeues, two-pointer advances), bound the total by counting how many times each item can be consumed, not by multiplying the loop bounds. This is amortised analysis.

## Where it shows up

- Next greater / next smaller element, daily temperatures
- Largest rectangle in a histogram
- Visibility and skyline sweeps, some convex hull variants

---

## Connections
- Sits with [[introduction to algorithms]], and inverts its warning about greed. The nearest neighbour tour is greedy and *wrong* — one counterexample sinks it, because a cheap local step can force an expensive leap later. This stack is greedy and *right*, and the licence is the invariant: once $\text{nums}[j] \le \text{nums}[i]$ with $j$ to the right of $i$, index $j$ can never be the answer for anything further left, so discarding it throws away nothing. Greed is safe exactly when "never needed again" is provable, not when it merely looks plausible.
- The direct counterpoint to [[basic recursion for sum loop]]. That note's $1 + 2 + \cdots + n = \frac{n(n+1)}{2}$ is the reason a nested loop is normally read as quadratic; here the same nesting is linear, because $\sum_{i=0}^{n-1} p_i$ is bounded by the $n$ pushes rather than evaluated as a sum over loop bounds. Two finite sums, two different ways of getting at them: closed form there, a counting bound here.
- That bound is an injection, in the sense of [[sets]]: every pop is paired with the one distinct push that put that index on the stack, so the set of pops is no larger than the set of pushes. Summation and set-builder notation are unpacked in that note.
- Same accounting, different problem: [[Bitset Auth]] expands each role exactly once in index order, so the closure costs one sweep and the request path is a single AND. Both notes pay a bounded price up front per item instead of re-deriving it in the inner loop — as [[Cylinder]] replaces a loop with a closed form per index.
- $-1$ as "there is no next greater element" is the identity-as-safe-default convention again: the all-zero id in [[Bit Encoding]], the empty OR in [[Bitset Auth]]. The stack itself holds indices rather than values for the same reason [[Bit Encoding]] stores a slot number — the index is the handle, and `St.top − i` is arithmetic on handles.
- The convex hull variants are where this meets the CAD notes. Andrew's monotone chain keeps a stack of hull vertices and pops while the last turn is not a left turn, and the sign of that turn is the $2 \times 2$ determinant of [[Determinant (cofactor) form of the cross product]] — the pop condition is a cross product rather than a comparison. The hull it builds is the one the control points of [[Bezier Curves]] are guaranteed to contain their curve inside.
- The container *is* the algorithm, as here: [[Breadth First Search]] with a FIFO queue explores in rings, and the same code with a LIFO stack is depth first search.
- The same counting bound on a graph instead of an array: [[kahns algorithm]] enqueues each node at most once and decrements each edge exactly once, giving $O(V + E)$ from the pushes rather than from the nesting.
- Part of [[Home]].

## Open
- [ ] Largest rectangle in a histogram worked through with the same invariant, including why each bar's width comes out of the pop.
- [ ] Monotone chain convex hull written out, with the turn test as the pop condition — the bridge to [[Determinant (cofactor) form of the cross product]].
- [ ] The queue twin: monotonic deque for sliding-window maximum, where elements leave at both ends and the amortised argument still holds.
