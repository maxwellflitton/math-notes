---
tags:
  - maths
  - linear-algebra
  - gaussian-elimination
  - matrices
date: 2026-10-25
---

# Linear algebra, row operations, and Gaussian elimination

We should already be familiar with the linear equation as the following generalisation:

$$
y = mx + c
$$

An equation involving $n$ variables $x_1, x_2, \dots, x_n$ is said to be linear in each of these variables if it can be written in the following form:

$$
a_1 x_1 + a_2 x_2 + a_3 x_3 + \dots + a_n x_n = d
$$

We can show that the $y = mx + c$ equation is linear by rewriting it as $y - mx = c$, where $n = 2$ variables and

$$
a_1 = 1, \quad x_1 = y, \quad a_2 = -m, \quad x_2 = x, \quad d = c
$$

## A simple system

Now let's look at the following simple system of linear equations:

$$
\underbrace{3x + 2y = 8}_{\alpha} \quad \text{and} \quad \underbrace{x - y = 1}_{\beta}
$$

We could eliminate $y$ by $\alpha + 2\beta$, because both equations are linear: $-2y + 2y$ cancels out. We would then have $5x = 10$ left as an equation. Once we have $x$ we can solve for $y$, which in this example is one.

## Three equations

We can show multiple linear equations with the following:

$$
\begin{aligned}
x_1 - 4x_2 + 2x_3 &= -9 && (E_1) \\
3x_1 - 2x_2 + 3x_3 &= 7 && (E_2) \\
8x_1 - 2x_2 + 9x_3 &= 34 && (E_3)
\end{aligned}
$$

We can collapse this system into two linear equations and remove $x_1$ with the following:

$$
\begin{aligned}
10x_2 - 3x_3 &= 34 && (E_{2a}) = (E_2) - 3(E_1) \\
30x_2 - 7x_3 &= 106 && (E_{3a}) = (E_3) - 8(E_1)
\end{aligned}
$$

We can then solve for $x_3$ with the following:

$$
2x_3 = 4 \qquad (E_{3b}) = (E_{3a}) - 3(E_{2a})
$$

You may have noticed that we have essentially created a "staircase" of equations. This is where $E_{3b}$ has just $x_3$, $E_{2a}$ has $x_2$ and $x_3$, so we have a dominoes effect where we can solve all $x_n$.

> [!tip] Solution
> $x_3 = 2$, then $10x_2 - 6 = 34 \Rightarrow x_2 = 4$, then $x_1 - 16 + 4 = -9 \Rightarrow x_1 = 3$. Check against $E_2$: $9 - 8 + 6 = 7$ ✓

### Why swapping equations is legal

We should also cover why swapping equations is legal. If $E_1$ had started with $0x_1$, where $a_1 = 0$, the first equation cannot help you eliminate $x_1$ from the others. The fix here is to swap $E_1$ with some equation that does have a non-zero $a_1$, and then carry on as before.

### Why adding a multiple of one equation to another is legal

We also have to ask the question: why does adding a multiple of one equation to another leave the solutions untouched? The answer is that if some numbers satisfy $E_1$ and $E_2$, then they satisfy any combination of them. You are adding two true statements, so nothing is lost going forward. You can also subtract the multiple off again, so it is reversible.

## Matrix form

Now we have clarified some rules, we can go back to our three linear equations and represent them in matrix form:

$$
\begin{bmatrix} 1 & -4 & 2 \\ 3 & -2 & 3 \\ 8 & -2 & 9 \end{bmatrix}
\begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}
=
\begin{bmatrix} -9 \\ 7 \\ 34 \end{bmatrix}
\;\Rightarrow\;
\begin{bmatrix} x_1 - 4x_2 + 2x_3 \\ 3x_1 - 2x_2 + 3x_3 \\ 8x_1 - 2x_2 + 9x_3 \end{bmatrix}
=
\begin{bmatrix} -9 \\ 7 \\ 34 \end{bmatrix}
$$

The matrix of the coefficients is the coefficient matrix, denoted as $A$. The unknown vector of $x$ variables can be denoted as $\mathbf{x}$, and the constant vector can be denoted as $\mathbf{b}$, giving the following equation:

$$
A\mathbf{x} = \mathbf{b}
$$

Here we can represent the above equation as an augmented matrix below:

$$
[A \mid \mathbf{b}] =
\left[\begin{array}{ccc|c}
1 & -4 & 2 & -9 \\
3 & -2 & 3 & 7 \\
8 & -2 & 9 & 34
\end{array}\right]
\begin{matrix} R_1 \\ R_2 \\ R_3 \end{matrix}
$$

There is no deep meaning, it's just new notation for expressing information. We can then achieve our staircase with the following, one step at a time:

**Step 1** — eliminate $x_1$ from $R_2$:

$$
\begin{matrix} \\ R_2 - 3R_1 \\ \end{matrix}
\left[\begin{array}{ccc|c}
1 & -4 & 2 & -9 \\
0 & 10 & -3 & 34 \\
8 & -2 & 9 & 34
\end{array}\right]
\begin{matrix} R_1 \\ R_{2a} \\ R_3 \end{matrix}
$$

**Step 2** — eliminate $x_1$ from $R_3$:

$$
\begin{matrix} \\ \\ R_3 - 8R_1 \end{matrix}
\left[\begin{array}{ccc|c}
1 & -4 & 2 & -9 \\
0 & 10 & -3 & 34 \\
0 & 30 & -7 & 106
\end{array}\right]
\begin{matrix} R_1 \\ R_{2a} \\ R_{3a} \end{matrix}
$$

**Step 3** — eliminate $x_2$ from $R_{3a}$:

$$
\begin{matrix} \\ \\ R_{3a} - 3R_{2a} \end{matrix}
\left[\begin{array}{ccc|c}
1 & -4 & 2 & -9 \\
0 & 10 & -3 & 34 \\
0 & 0 & 2 & 4
\end{array}\right]
\begin{matrix} R_1 \\ R_{2a} \\ R_{3b} \end{matrix}
$$

This is exactly the staircase from $E_1, E_{2a}, E_{3b}$ above, just written as rows.

### Allowed row operations

What we can do is the following row operations:

- Interchange any two complete rows: $R_i \leftrightarrow R_j$
- Multiply each element in a row by the same constant: $R_i \to \lambda R_i$
- Add (or subtract) a constant multiple of one row to (or from) another row: $R_i \to R_i \pm \lambda R_j$

We must also note that the staircase method is called **back substitution**.

## Gaussian elimination

The back substitution is an example of Gaussian elimination. However, there are situations. First let's show the process in a simple system by solving the following equations:

$$
\begin{aligned}
x_1 + x_2 - x_3 &= 2 \\
5x_1 + 2x_2 + 2x_3 &= 20 \\
4x_1 - 2x_2 - 3x_3 &= 15
\end{aligned}
\;\Rightarrow\;
\left[\begin{array}{ccc|c}
1 & 1 & -1 & 2 \\
5 & 2 & 2 & 20 \\
4 & -2 & -3 & 15
\end{array}\right]
\begin{matrix} R_1 \\ R_2 \\ R_3 \end{matrix}
$$

**Step 1** — $R_2 - 5R_1$ and $R_3 - 4R_1$:

$$
\begin{matrix} \\ R_2 - 5R_1 \\ R_3 - 4R_1 \end{matrix}
\left[\begin{array}{ccc|c}
1 & 1 & -1 & 2 \\
0 & -3 & 7 & 10 \\
0 & -6 & 1 & 7
\end{array}\right]
\begin{matrix} R_1 \\ R_{2a} \\ R_{3a} \end{matrix}
$$

(Working: $R_2 - 5R_1 = (5-5,\; 2-5,\; 2+5 \mid 20-10) = (0, -3, 7 \mid 10)$ and $R_3 - 4R_1 = (4-4,\; -2-4,\; -3+4 \mid 15-8) = (0, -6, 1 \mid 7)$.)

**Step 2** — $R_{3a} - 2R_{2a}$:

$$
\begin{matrix} \\ \\ R_{3a} - 2R_{2a} \end{matrix}
\left[\begin{array}{ccc|c}
1 & 1 & -1 & 2 \\
0 & -3 & 7 & 10 \\
0 & 0 & -13 & -13
\end{array}\right]
\begin{matrix} R_1 \\ R_{2a} \\ R_{3b} \end{matrix}
$$

(Working: $(0,\; -6+6,\; 1-14 \mid 7-20) = (0, 0, -13 \mid -13)$.)

Our final matrix form gives the following equations:

$$
\begin{aligned}
x_1 + x_2 - x_3 &= 2 \\
-3x_2 + 7x_3 &= 10 \\
-13x_3 &= -13
\end{aligned}
$$

We then have the equations that can be easily solved for. This is a simple case, but it makes sense to rearrange the order of the rows if $x_1$ for $R_1$ is zero, or if the rearrangement of the rows results in simpler operations.

> [!tip] Solution
> $x_3 = 1$, then $-3x_2 + 7 = 10 \Rightarrow x_2 = -1$, then $x_1 - 1 - 1 = 2 \Rightarrow x_1 = 4$. Check: $5(4) + 2(-1) + 2(1) = 20$ ✓

### You don't always need a lower triangle of zeros

We must also note that we do not need a lower triangle of zeros to solve the equations. Below is a good example:

$$
\begin{matrix} \\ R_2 - 2R_3 \\ \end{matrix}
\left[\begin{array}{ccc|c}
0 & 0 & 1 & 2 \\
2 & 1 & 0 & 5 \\
1 & 2 & 0 & 7
\end{array}\right]
\begin{matrix} R_1 \\ R_2 \\ R_3 \end{matrix}
\;\Rightarrow\;
\left[\begin{array}{ccc|c}
0 & 0 & 1 & 2 \\
0 & -3 & 0 & -9 \\
1 & 2 & 0 & 7
\end{array}\right]
\begin{matrix} R_1 \\ R_{2a} \\ R_3 \end{matrix}
$$

We can see in $R_1$ straight away that $x_3 = 2$, and $R_{2a}$ instantly gives $x_2 = 3$. We can plug $x_2$ into $R_3$ to get $x_1 = 1$.

## The inverse matrix

There is a powerful benefit to performing Gaussian elimination in matrix form, and this is the inverse matrix:

$$
\text{if } \det A \neq 0 \;\Rightarrow\; A A^{-1} = I
$$

So in the non-singular case, the unique solution of a system of equations $A\mathbf{x} = \mathbf{b}$ is given by:

$$
\mathbf{x} = A^{-1}\mathbf{b}
$$

However, there is a limitation to this. As the matrix grows, finding the inverse requires more steps. There is the exception of sparse matrices, but the inverse should not be thoughtlessly applied.

> [!note] Correction on cost
> The handwritten version says the inverse grows "to the $n!$ multiplications". That's the cost of computing a determinant (and hence the inverse) by cofactor expansion. In practice nobody does it that way: inverting via Gaussian elimination is $O(n^3)$, and solving $A\mathbf{x} = \mathbf{b}$ directly by elimination plus back substitution is cheaper still (roughly a third of the work) and numerically better behaved. So the practical rule is the one you landed on — don't form $A^{-1}$ just to solve a system; eliminate instead. The last sentence originally read "Gaussian elimination should not be thoughtlessly applied" but from context it's the _inverse_ that's the expensive thing, so I've reworded it.

## When the determinant is zero

Finally we can explore when the determinant is zero with the following equations:

$$
x + y = 4 \quad \text{and} \quad 2x + 2y = 5 \;\Rightarrow\; \begin{bmatrix} 1 & 1 \\ 2 & 2 \end{bmatrix} \;\Rightarrow\; \det = 2 - 2 = 0
$$

This means that the system is said to be **singular**, as there is no unique solution. It may have no solution or infinitely many solutions.

> [!note]
> This particular example has _no_ solution: the second equation says $x + y = 2.5$, which contradicts the first. If it had been $2x + 2y = 8$ the same determinant would be zero but there would be infinitely many solutions — the two lines coincide.

---

## Connections

- Part of [[Linear Algebra]], and the practical half of [[Determinants and inverses for matrices]]. That note computes $\det$ and $A^{-1}$ by cofactors, which costs $n!$ terms; elimination gets the same answers in $O(n^3)$ by making the matrix triangular, where the determinant is just the product of the pivots (times $-1$ per row swap). The correction callout above is exactly that gap between the textbook formula and the practical algorithm.
- $A\mathbf{x} = \mathbf{b}$ is the matrix product of [[Matrix algebra]] read as a system: row $i$ of $A$ dotted with $\mathbf{x}$ is equation $i$, which is why a row operation is an operation on one whole equation. The three allowed operations are themselves matrices — each is an invertible elementary matrix, so elimination is a product $E_k \cdots E_1 A = U$, i.e. the composition of transformations in [[Matrix Overview]] with $\det E \neq 0$ at every step, which is why the solution set never changes.
- $\mathbf{x} = A^{-1}\mathbf{b}$ needs $\det A \neq 0$, the invertibility condition of [[Determinants and inverses for matrices]]. Geometrically $\det = 0$ means the rows span no area — the degenerate $2 \times 2$ determinant of [[Determinant (cofactor) form of the cross product]], and at $3 \times 3$ the coplanar test of [[Volumes and the scalar triple product]]. Elimination detects it concretely: a row of zeros on the left is a collapsed span.
- The singular example _is_ two lines. $x + y = 4$ and $2x + 2y = 5$ are parallel in the sense of [[Lines]] and [[Euclidean space]] — same direction vector, different intercept, so they never meet and there is no solution; change the constant to $8$ and they coincide, giving infinitely many. The three outcomes are the three possible solution sets of [[sets]]: empty, a single point, or an infinite family.
- A zero pivot forcing a row swap is the same degeneracy that makes $\det(A - \lambda I) = 0$ interesting: solving that system by elimination is how an eigenvector is actually found, with the free variable after elimination being the eigenvector's scale — [[eigen]].
- Cost, in the sense of [[introduction to algorithms]]: elimination is correct _and_ affordable, where cofactor expansion is correct and not. The count is a sum of squares, $\sum_{k=1}^{n} k^2 \approx n^3/3$ operations, the same kind of closed form for a nested loop as $1 + 2 + \cdots + n$ in [[basic recursion for sum loop]] — one dimension up because elimination loops over rows, columns and the right-hand side.
- "Do not form $A^{-1}$ just to solve a system" has an exception worth naming: in [[Ortho Application#7. Running it backwards]] the inverse of $PV$ is built once and reused for every picked pixel, so the inversion is amortised over many right-hand sides. The rule is about solving once, not about never inverting.
- Part of [[Home]].

## Open

- [ ] LU decomposition: storing the multipliers instead of discarding them, so repeated $\mathbf{b}$ values reuse one elimination.
- [ ] Partial pivoting for numerical stability — swapping for the _largest_ pivot rather than merely a non-zero one, and what catastrophic cancellation looks like.
- [ ] The determinant read off the triangular form as a product of pivots, with the sign bookkeeping for row swaps.
- [ ] Reduced row echelon form and the Gauss–Jordan route to $A^{-1}$ by eliminating $[A \mid I]$ into $[I \mid A^{-1}]$.
- [ ] Rank, free variables, and writing the infinite solution family in parametric form when $\det A = 0$.
