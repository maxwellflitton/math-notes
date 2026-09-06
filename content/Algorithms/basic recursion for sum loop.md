# Deriving the Closed Form of a Sum Loop

Tags: #algorithms #induction #recurrences #skiena

We want the closed form of the sum of an ordered array of integers `[1, 2, 3, ..., n]`.

Write $S(n)$ for that sum.

---

## 1. The recurrence

A sum can be written in terms of a smaller sum:

$$\sum_{i=1}^{n} i = n + \sum_{i=1}^{n-1} i$$

This is the mathematical way of denoting recursion — the right-hand side contains the same kind of object as the left, just smaller.

In the $S(n)$ notation, together with its base case:

$$S(n) = n + S(n-1), \qquad S(1) = 1$$

**The base case is not optional.** Without it the recurrence is true but defines nothing — it just defers forever, the same way a recursive function without a terminating branch never returns.

This maps line-for-line onto code:

```rust
fn sum(n: u64) -> u64 {
    if n == 1 {
        1                   // base case
    } else {
        n + sum(n - 1)      // general case
    }
}
```

---

## 2. Unrolling

Apply the recurrence to itself repeatedly:

$$S(n) = \underbrace{n}_{k=0} + \underbrace{(n-1)}_{k=1} + \underbrace{(n-2)}_{k=2} + \underbrace{(n-3)}_{k=3} + \cdots + \underbrace{\big(n-(n-1)\big)}_{k=n-1}$$

Each term has the form $n - k$.

**Where it stops.** The base case is $S(1) = 1$, so the last term is $1$. Setting the general term equal to it:

$$n - k = 1 \implies k = n - 1$$

So $k$ runs $0, 1, 2, \ldots, n-1$ — that is $n$ values, so there are **$n$ terms**. Keeping the last term written as $n - (n-1)$ rather than simplifying it to $1$ is deliberate: it makes the term count visible, and the count is what the next step needs.

### Unrolling alone is not enough

The unrolled expression is just the original sum written in descending order. Collecting terms doesn't help either, because the *number* of terms is itself $n$ — it isn't a fixed count you can multiply out. A new idea is needed.

---

## 3. Regrouping into a second equation

Split every term into its $n$ part and its constant part:

$$S(n) = \underbrace{n + n + \cdots + n}_{n \text{ copies}} \;-\; \underbrace{\big(0 + 1 + 2 + \cdots + (n-1)\big)}_{\text{the constants}}$$

- $n$ copies of $n$ is $n \times n = n^2$
- the constants are $1 + 2 + \cdots + (n-1)$, which is exactly $S(n-1)$

So:

$$S(n) = n^2 - S(n-1)$$

**Why this counts as a second equation.** Both this and the recurrence express $S(n)$ in terms of $S(n-1)$, but they were derived differently and neither is a rearrangement of the other:

| Equation | How it splits the sum |
|---|---|
| $S(n) = n + S(n-1)$ | peels off the largest term, leaves the rest |
| $S(n) = n^2 - S(n-1)$ | keeps all $n$ terms, splits each into a part and a correction |

---

## 4. Eliminating the recursive term

Two equations, two unknowns:

$$
\begin{aligned}
S(n) &= n^2 - S(n-1) \\
S(n) &= n + S(n-1)
\end{aligned}
$$

$S(n-1)$ appears with opposite signs, so **adding** the equations cancels it:

$$2S(n) = n^2 + n$$

$$S(n) = \frac{n^2 + n}{2} = \frac{n(n+1)}{2}$$

**Check.** $n=4$: $\frac{4 \cdot 5}{2} = 10$, and $1+2+3+4 = 10$ ✓ — $n=5$: $\frac{5 \cdot 6}{2} = 15$, and $1+2+3+4+5 = 15$ ✓

> **The general move:** when stuck with an unresolved recursive term, look for a *second* expression containing it. One equation in two unknowns is a dead end; two lets you eliminate.

### This is the Gauss pairing in disguise

Writing the sum both ways and adding is the same trick as stacking the ascending and descending orders:

$$
S = 1 + 2 + \cdots + n
$$
$$
S = n + (n-1) + \cdots + 1
$$

Each column totals $n+1$, and there are $n$ columns, so $2S = n(n+1)$.

---

## 5. Verifying by induction

The derivation above *discovered* the formula. Induction is the separate step that *proves* it holds for every $n$ — it verifies a candidate, it doesn't produce one.

The claim is infinitely many statements, one per $n$. Induction gets all of them from two finite pieces of work.

### Basis case

Check the claim outright at the smallest $n$:

$$S(1) = 1, \qquad \frac{1 \cdot 2}{2} = 1 \quad ✓$$

This corresponds to the terminating branch of the recursive function.

### General assumption (the induction hypothesis)

Assume the claim holds for some particular $n-1$:

$$S(n-1) = \frac{(n-1)n}{2}$$

This is **not** assuming what we're trying to prove. It's the antecedent of an implication we're about to prove. In code terms, it's the assumption that the recursive call returns the right answer — the trust you extend to `sum(n - 1)` while you're still writing `sum`.

### General case

Show the claim then follows at $n$. Substitute the hypothesis into the recurrence:

$$S(n) = n + S(n-1) = n + \frac{(n-1)n}{2}$$

$$= \frac{2n + n^2 - n}{2} = \frac{n^2 + n}{2} = \frac{n(n+1)}{2}$$

which is the claim at $n$. ∎

This corresponds to the recursive branch of the function.

### Why the two pieces suffice

The basis case establishes $n=1$. The general case is an implication proved by algebra valid for *every* $n$ at once. Chaining them: true at 1, so true at 2, so true at 3, and so on. For any $n$ you name there is a finite chain of implications reaching it. The basis case is what discharges the hypothesis.

> **Recursion and induction are the same three-part shape:** basis case ↔ terminating branch; general assumption ↔ trusting the recursive call; general case ↔ recursive branch. This is why proving a recursive algorithm correct is usually a matter of pointing induction at it.

---

## 6. Why any of this matters for code

The closed form proves that these two compute the same value:

```rust
fn sum_loop(n: u64) -> u64 {      // O(n)
    (1..=n).sum()
}

fn sum_closed(n: u64) -> u64 {    // O(1)
    n * (n + 1) / 2
}
```

That equivalence is what licenses swapping the linear loop for the constant-time formula. At $n = 10^9$ that's a billion additions versus three operations.

**Caveats:**
- The division is always exact — $n$ and $n+1$ are consecutive, so one is even. Multiply first, then divide.
- `sum_closed` overflows `u64` when $n(n+1) > 2^{64}$, around $n \approx 4.3 \times 10^9$ — *earlier* than the result itself would overflow.
- The recursive version recurses $n$ deep and will blow the stack around $n \approx 10^5$; Rust does not guarantee tail-call elimination. It mirrors the proof, it isn't for production.
- `sum(0)` computes `0 - 1` on a `u64` — panics in debug, wraps in release. Either take the base case to `n == 0 => 0` or guard the input.

### Testing is not proving

Checking the two agree for $n = 1 \ldots 20$ would catch a typo like `n * (n - 1) / 2`, but no finite number of passing cases rules out failure at some larger $n$. That gap is exactly what induction closes — and the point of the slide's first line: *failure to find a counterexample does not mean the algorithm is correct*.

---

## 7. Notational aside: empty sums

The cleaner mathematical convention defines the empty sum as zero:

$$\sum_{i=1}^{0} i = 0$$

With that, the recurrence works right down to $n=1$: it gives $1 = 1 + 0$, and no special case is needed. Mathematicians usually anchor at $0$ for this reason; the Rust above anchors at $1$ instead, which is why it needs the guard.

---

## Where this shows up

- A nested loop whose inner loop runs to the outer index does $1 + 2 + \cdots + n$ units of work — the closed form is how you get from "two nested loops" to "quadratic".
- Same sum counts the pairs in a set of $n$ things: handshakes, or the pairwise distance comparisons in a TSP tour.
## Connections
- Part of the same area as [[introduction to algorithms]]. That note's "the best way to show an algorithm is wrong is to show one example where it doesn't work" is the flip side of §5 here: a counterexample refutes, but no number of passing cases proves — induction is what closes that gap.
- The pair count $1 + 2 + \cdots + n = \frac{n(n+1)}{2}$ is what the exhaustive TSP search in [[introduction to algorithms]] is summing over when it compares points pairwise, and the $\sum_{k=1}^{n-1}$ in its cost function is the same shape of finite sum.
- Recursion with a base case is the shape of de Casteljau's algorithm in [[Bezier Curves]] (repeated linear interpolation down to a point) and the Cox–de Boor recursion in [[B-Splines and NURBS]] — both are recurrences whose termination is the whole reason they evaluate.
- $S(n) = \frac{n(n+1)}{2}$ is the discrete twin of $\int_0^n x\,dx = \frac{n^2}{2}$ — summing unit steps versus integrating continuously. See [[Calculus]] and [[Direct Integration]].
- A recurrence *is* a difference equation: shrink the step and $\frac{p(t + \delta t) - p(t)}{\delta t}$ becomes $\frac{dp}{dt}$, which is how [[First order differential equations]] gets started — the limit itself is [[lim-zero-example]].
- Summation and set-builder notation are unpacked in [[sets]]; the index algebra behind $\frac{2n + n^2 - n}{2}$ is [[exp-algebria-rules]].
