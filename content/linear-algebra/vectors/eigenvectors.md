---
tags: [maths, linear-algebra, eigenvectors, MST224]
source: OU Unit 5 Linear Algebra, Section 2 (Introducing eigenvalues and eigenvectors)
related: "[[eigen]]"
created: 2026-11-08
---
# Eigenvectors and Eigenvalues

As mentioned in [[eigen]], an eigenvalue is a number that multiplies a vector but doesn't change its nature (its direction). Before exploring eigenvalues and eigenvectors in more detail, it is worth looking at a population growth model, because that is where the idea falls out naturally.

## 1. Population model (Exton and Wyeville)

For a simple model we assume births and deaths cancel each other out, so we only focus on net migration between the two towns. The next year's populations are given by

$$
\begin{aligned}
x_{n+1} &= 0.9x_n + 0.2y_n \\
y_{n+1} &= 0.1x_n + 0.8y_n
\end{aligned}
\quad\Longrightarrow\quad
\begin{bmatrix} x_{n+1} \\ y_{n+1} \end{bmatrix}
=
\begin{bmatrix} 0.9 & 0.2 \\ 0.1 & 0.8 \end{bmatrix}
\begin{bmatrix} x_n \\ y_n \end{bmatrix}
$$

We can write this as $\mathbf{x}_{n+1} = \mathbf{T}\mathbf{x}_n$ where $\mathbf{T}$ is the **transition matrix**.

Now something interesting happens when we iterate through the years. Say $x_0 = 10\,000$ and $y_0 = 8\,000$:

$$
\mathbf{x}_1 =
\begin{bmatrix} 0.9 & 0.2 \\ 0.1 & 0.8 \end{bmatrix}
\begin{bmatrix} 10\,000 \\ 8\,000 \end{bmatrix}
=
\begin{bmatrix} 10\,600 \\ 7\,400 \end{bmatrix}
$$

Then another year:

$$
\mathbf{x}_2 =
\begin{bmatrix} 0.9 & 0.2 \\ 0.1 & 0.8 \end{bmatrix}
\begin{bmatrix} 10\,600 \\ 7\,400 \end{bmatrix}
=
\begin{bmatrix} 11\,020 \\ 6\,980 \end{bmatrix}
$$

Since $\mathbf{x}_1 = \mathbf{T}\mathbf{x}_0$ and $\mathbf{x}_2 = \mathbf{T}\mathbf{x}_1 = \mathbf{T}(\mathbf{T}\mathbf{x}_0) = \mathbf{T}^2\mathbf{x}_0$, we can see the pattern:

$$
\boxed{\mathbf{x}_n = \mathbf{T}^n \mathbf{x}_0}
$$

For the populations, they initially diverge but then stabilise from about 15 years on. By year 29 nothing is changing any more:

$$
\mathbf{x}_{29} = \mathbf{x}_{30} = \mathbf{x}_{31} = \begin{bmatrix} 12\,000 \\ 6\,000 \end{bmatrix}
$$

With every iteration past 29 we can say $\mathbf{T}\mathbf{x} = \mathbf{x}$. $\mathbf{T}$ leaves $\mathbf{x}$ unchanged, so $\mathbf{x}$ is an **eigenvector** of $\mathbf{T}$. The general expression is

$$
\mathbf{A}\mathbf{v} = \lambda\mathbf{v}
$$

where $\lambda$ is a constant scalar, the **eigenvalue**. For our population example $\mathbf{T}\mathbf{x} = 1\mathbf{x}$, so $\lambda = 1$.

## 2. Eigenvalues and eigenvectors in 2D

Consider

$$
\mathbf{A}\mathbf{v} =
\begin{bmatrix} 3 & 2 \\ 1 & 4 \end{bmatrix}
\begin{bmatrix} 1 \\ 1 \end{bmatrix}
=
\begin{bmatrix} 5 \\ 5 \end{bmatrix}
= 5 \begin{bmatrix} 1 \\ 1 \end{bmatrix}
$$

$\mathbf{A}$ is the transformation. $\mathbf{v} = [1 \;\; 1]^T$ is an eigenvector and $5$ is its eigenvalue, because the direction of the vector is not changed, it is just scaled by the eigenvalue.

For contrast, $[1 \;\; 0]^T$ is **not** an eigenvector of this $\mathbf{A}$:

$$
\begin{bmatrix} 3 & 2 \\ 1 & 4 \end{bmatrix}
\begin{bmatrix} 1 \\ 0 \end{bmatrix}
=
\begin{bmatrix} 3 \\ 1 \end{bmatrix}
$$

The output points in a different direction, so no single scalar can get you from input to output.

Another eigenvector of $\mathbf{A}$ is $[-2 \;\; 1]^T$ with eigenvalue $2$:

$$
\begin{bmatrix} 3 & 2 \\ 1 & 4 \end{bmatrix}
\begin{bmatrix} -2 \\ 1 \end{bmatrix}
=
\begin{bmatrix} -4 \\ 2 \end{bmatrix}
= 2 \begin{bmatrix} -2 \\ 1 \end{bmatrix}
$$

### Complex eigenvectors

We can also have complex eigenvectors and eigenvalues:

$$
\begin{bmatrix} 3 & -2 \\ 4 & -1 \end{bmatrix}
\begin{bmatrix} 1 \\ 1 - i \end{bmatrix}
=
\begin{bmatrix} 3 - 2 + 2i \\ 4 - 1 + i \end{bmatrix}
=
\begin{bmatrix} 1 + 2i \\ 3 + i \end{bmatrix}
= (1 + 2i) \begin{bmatrix} 1 \\ 1 - i \end{bmatrix}
$$

(Check: $(1+2i)(1-i) = 1 - i + 2i - 2i^2 = 3 + i$. ✓) Real matrices that rotate tend to produce complex eigenvalues; matrices that scale or reflect produce real ones.

Essentially our eigenvalue equation is defined by

$$
\mathbf{A}\mathbf{v} = \lambda\mathbf{v}
$$

## 3. Linear dependence

Before we move further with eigenvectors and eigenvalues, we must explore linear dependence. Say we have three vectors. They are **linearly dependent** if

$$
\alpha_1\mathbf{v}_1 + \alpha_2\mathbf{v}_2 + \alpha_3\mathbf{v}_3 = \mathbf{0}
$$

has a solution other than $\alpha_1 = \alpha_2 = \alpha_3 = 0$. If the only solution is all zeros, they are **linearly independent**.

For example, look at the unit vectors:

$$
\alpha_1 \begin{bmatrix} 1 \\ 0 \end{bmatrix} + \alpha_2 \begin{bmatrix} 0 \\ 1 \end{bmatrix} = \mathbf{0}
$$

Intuitively, $\hat{\mathbf{x}}$ and $\hat{\mathbf{y}}$ don't share a plane of action, so they are linearly independent. There is no non-zero combination of $\alpha_1$ and $\alpha_2$ that gives zero because $\hat{\mathbf{x}}$ and $\hat{\mathbf{y}}$ have no effect on each other.

Now look at three vectors:

$$
\mathbf{i} = \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}, \quad
\mathbf{j} = \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}, \quad
\mathbf{v} = \begin{bmatrix} 3 \\ 2 \\ 0 \end{bmatrix}
$$

With $\mathbf{v}_1 = \mathbf{v}$, $\mathbf{v}_2 = \mathbf{i}$, $\mathbf{v}_3 = \mathbf{j}$ we get $\alpha_1 = 1$, $\alpha_2 = -3$, $\alpha_3 = -2$:

$$
1\begin{bmatrix} 3 \\ 2 \\ 0 \end{bmatrix} - 3\begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix} - 2\begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix} = \mathbf{0}
$$

so these three are linearly dependent. $\mathbf{v}$ is just built out of $\mathbf{i}$ and $\mathbf{j}$.

Linear independence is connected with dimension. In 2D space, if we have two vectors there is a chance they are linearly independent. If we have three vectors in 2D, the third will always be some combination of the first two (unless the first two were already dependent), so we can always find non-zero $\alpha$'s that sum to zero. **The dimension of a space is the maximum number of linearly independent vectors that are possible in it.**

### Writing one vector in terms of others

We can also see this play out in matrix form. Say we have

$$
\mathbf{v}_1 = \begin{bmatrix} 1 \\ 1 \end{bmatrix}, \quad
\mathbf{v}_2 = \begin{bmatrix} -2 \\ 1 \end{bmatrix}, \quad
\mathbf{v} = \begin{bmatrix} 1 \\ 3 \end{bmatrix}
$$

We can express $\mathbf{v}$ as a linear combination of $\mathbf{v}_1$ and $\mathbf{v}_2$:

$$
\begin{bmatrix} 1 \\ 3 \end{bmatrix} = \alpha \begin{bmatrix} 1 \\ 1 \end{bmatrix} + \beta \begin{bmatrix} -2 \\ 1 \end{bmatrix}
$$

This gives the simultaneous equations

$$
\begin{aligned}
1 &= \alpha - 2\beta \\
3 &= \alpha + \beta
\end{aligned}
$$

Double the second and add: $1 + 6 = 3\alpha \Rightarrow \alpha = \tfrac{7}{3}$. Put $\alpha$ into the bottom equation: $3 = \tfrac{7}{3} + \beta \Rightarrow \beta = \tfrac{2}{3}$.

So $\mathbf{v} = \tfrac{7}{3}\mathbf{v}_1 + \tfrac{2}{3}\mathbf{v}_2$. Because $\mathbf{v}_1$ and $\mathbf{v}_2$ are linearly independent and we are in 2D, *any* vector in the plane can be written this way, and the $\alpha, \beta$ are unique. That is what makes $\{\mathbf{v}_1, \mathbf{v}_2\}$ a **basis**.

## 4. Why linear independence matters for eigenvectors

So why are we bothering with linear independence for eigenvalues and eigenvectors?

**Eigenvectors with different eigenvalues are always linearly independent.** If they were dependent, one would be a combination of the others, and applying $\mathbf{A}$ would scale the pieces by different amounts, so the combination couldn't itself be scaled by a single $\lambda$ — it wouldn't be eigen.

We can technically have infinite eigenvectors for one eigenvalue, because any scalar multiple of an eigenvector points in the same direction and is also an eigenvector. That is why the statement is about *directions*: eigenvectors with **distinct** eigenvalues are always linearly independent.

If we are working in 2D we can only have at most 2 linearly independent eigenvectors. More generally, an $n \times n$ matrix has **at most** $n$ linearly independent eigenvectors. Distinct eigenvalues guarantee a full set of $n$; a repeated eigenvalue can give fewer, as in

$$\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}$$

which has eigenvalue $1$ twice but only one eigenvector direction, $[1 \;\; 0]^T$. That is why the result in the next section is stated conditionally: *if* $\mathbf{A}$ has $n$ linearly independent eigenvectors.

## 5. Eigenvector expansion

Now that we have linear independence covered, we can move on to eigenvector expansion. If $\mathbf{A}$ is an $n \times n$ matrix with $n$ linearly independent eigenvectors $\mathbf{v}_1, \dots, \mathbf{v}_n$, then any vector $\mathbf{v}$ can be written as

$$
\mathbf{v} = c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_n\mathbf{v}_n
$$

This is the same change-of-basis trick as Section 3, just using the eigenvectors as the basis.

We remember that eigenvectors don't change direction, they just scale. So once a vector is written in the eigenvector basis, applying $\mathbf{A}$ doesn't need matrix multiplication at all:

$$
\mathbf{A}\mathbf{v} = \mathbf{A}(c_1\mathbf{v}_1 + c_2\mathbf{v}_2) = c_1\mathbf{A}\mathbf{v}_1 + c_2\mathbf{A}\mathbf{v}_2 = c_1\lambda_1\mathbf{v}_1 + c_2\lambda_2\mathbf{v}_2
$$

Linearity splits $\mathbf{A}$ across the sum, then each $\mathbf{A}\mathbf{v}_i$ is replaced by $\lambda_i\mathbf{v}_i$. We multiply by the eigenvalues because that is what the matrix *does* to each eigenvector — it isn't a step we chose, it's what applying $\mathbf{A}$ turns into in this basis.

### Worked example (Exercise 19)

$$
\mathbf{A} = \begin{bmatrix} 3 & 2 \\ 1 & 4 \end{bmatrix}, \quad
\mathbf{v}_1 = \begin{bmatrix} 1 \\ 1 \end{bmatrix}, \quad
\mathbf{v}_2 = \begin{bmatrix} -2 \\ 1 \end{bmatrix}, \quad
\lambda_1 = 5, \quad \lambda_2 = 2, \quad
\mathbf{r}_0 = \begin{bmatrix} -2 \\ 4 \end{bmatrix}
$$

Like with differential equations, we have some constants we can solve for with initial conditions:

$$
\mathbf{r}_0 = c_1\mathbf{v}_1 + c_2\mathbf{v}_2
\;\Rightarrow\;
\begin{bmatrix} -2 \\ 4 \end{bmatrix} = c_1\begin{bmatrix} 1 \\ 1 \end{bmatrix} + c_2\begin{bmatrix} -2 \\ 1 \end{bmatrix}
$$

This gives the linear equations

$$
\begin{aligned}
-2 &= c_1 - 2c_2 \\
4 &= c_1 + c_2
\end{aligned}
$$

Subtract the first from the second: $6 = 3c_2 \Rightarrow c_2 = 2$. Then $c_1 + 2 = 4 \Rightarrow c_1 = 2$. So

$$
\begin{bmatrix} -2 \\ 4 \end{bmatrix} = 2\begin{bmatrix} 1 \\ 1 \end{bmatrix} + 2\begin{bmatrix} -2 \\ 1 \end{bmatrix}
$$

These equations could be anything — populations of two cities, geometric transforms. We have essentially solved the $c$ constants for the initial values $\mathbf{r}_0$. The $c$'s are the coordinates of $\mathbf{r}_0$ in the eigenvector basis; they are fixed once and never change afterwards.

Now if we multiply the initial values by the matrix $\mathbf{A}$ we get an eigenvalue multiple of the eigenvectors:

$$
\mathbf{A}\mathbf{r}_0 = 2\lambda_1\mathbf{v}_1 + 2\lambda_2\mathbf{v}_2
= 2 \cdot 5\begin{bmatrix} 1 \\ 1 \end{bmatrix} + 2 \cdot 2\begin{bmatrix} -2 \\ 1 \end{bmatrix}
= \begin{bmatrix} 10 \\ 10 \end{bmatrix} + \begin{bmatrix} -8 \\ 4 \end{bmatrix}
= \begin{bmatrix} 2 \\ 14 \end{bmatrix}
$$

Check with direct matrix multiplication:

$$
\mathbf{A}\mathbf{r}_0 = \begin{bmatrix} 3 & 2 \\ 1 & 4 \end{bmatrix}\begin{bmatrix} -2 \\ 4 \end{bmatrix} = \begin{bmatrix} 2 \\ 14 \end{bmatrix} \quad ✓
$$

We can then multiply again and it will still hold:

$$
\mathbf{A}\mathbf{A}\mathbf{r}_0 = 2\lambda_1\lambda_1\mathbf{v}_1 + 2\lambda_2\lambda_2\mathbf{v}_2
\;\Rightarrow\;
\mathbf{A}^2\mathbf{r}_0 = 2\lambda_1^2\mathbf{v}_1 + 2\lambda_2^2\mathbf{v}_2
$$

With this we can conclude that

$$
\boxed{\mathbf{A}^k\mathbf{r}_0 = c_1\lambda_1^k\mathbf{v}_1 + c_2\lambda_2^k\mathbf{v}_2}
$$

Directions fixed, coefficients evolve. The $c$'s say where you start; the $\lambda^k$'s say how fast each component grows or shrinks from there. $\mathbf{A}^{100}\mathbf{r}_0$ costs two scalar powers, not a hundred matrix multiplies.

## 6. Convergence towards an eigenvector

In the general case, with $\mathbf{x} = c_1\mathbf{v}_1 + \cdots + c_n\mathbf{v}_n$,

$$
\mathbf{A}^k\mathbf{x} = c_1\lambda_1^k\mathbf{v}_1 + c_2\lambda_2^k\mathbf{v}_2 + \cdots + c_n\lambda_n^k\mathbf{v}_n
$$

As $k$ gets larger, the eigenvalue with the **largest modulus** dominates. If $\lambda_p$ is that eigenvalue, then

$$
\mathbf{A}^k\mathbf{x} \simeq c_p\lambda_p^k\mathbf{v}_p \quad \text{for large } k
$$

So for (almost) any starting vector, $\mathbf{A}^k\mathbf{x}$ ends up **proportional to** the dominant eigenvector. Ignoring scaling, repeated application of $\mathbf{A}$ converges to $\mathbf{v}_p$.

Key points:

- It converges to the same **direction**, not the same value. The matrix dictates the shape of the long-run answer ($\mathbf{v}_p$); the starting vector dictates the scale ($c_p$).
- $|\lambda_p| > 1$: blows up along $\mathbf{v}_p$. $|\lambda_p| = 1$: settles to a fixed vector $c_p\mathbf{v}_p$. $|\lambda_p| < 1$: shrinks to zero.
- The caveat "almost any" means $c_p \neq 0$. If the starting vector has no component along $\mathbf{v}_p$, it can never pick one up.

### Back to the population model

This is exactly what happened in Section 1. For $\mathbf{T}$, the eigenvectors are $\mathbf{v}_1 = [2 \;\; 1]^T$ with $\lambda_1 = 1$ and $\mathbf{v}_2 = [1 \;\; {-1}]^T$ with $\lambda_2 = 0.7$, and

$$
\mathbf{x}_0 = \begin{bmatrix} 10\,000 \\ 8\,000 \end{bmatrix} = 6000\mathbf{v}_1 - 2000\mathbf{v}_2
$$

so

$$
\mathbf{x}_k = 6000 \cdot 1^k\,\mathbf{v}_1 - 2000 \cdot 0.7^k\,\mathbf{v}_2
$$

$0.7^{30} \approx 0.0000225$, so the $\mathbf{v}_2$ contribution dies and

$$
\mathbf{x}_k \simeq 6000\mathbf{v}_1 = \begin{bmatrix} 12\,000 \\ 6\,000 \end{bmatrix}
$$

which is the 12 000 / 6 000 we found by brute-force iterating. A different starting population gives a different $c_1$, so a different total, but always the same 2:1 ratio between the towns.

This observation — repeatedly multiply by $\mathbf{A}$ and the result swings round to the dominant eigenvector — is the basis of numerical algorithms for finding eigenvectors of very large matrices (the power method), and of Google's PageRank.

## Connections

- The concrete half of [[eigen]]. That note states the idea in the abstract — the directions an operator only scales, and what a decomposition buys you — while this one arrives at it from a population model and then pushes it through to $\mathbf{A}^k$ and the long-run limit. Its remark that only the *direction* is fixed is §4 here: a whole line of eigenvectors per eigenvalue, which is why the statement about independence is about distinct eigenvalues, and why in practice they are normalised ([[Unit vectors]]).
- Section 3 is basis and dimension in the sense of [[Euclidean space]] and [[Basic Operations]]: a basis is a maximal linearly independent set, dimension is how many that is, and the uniqueness of $\alpha, \beta$ is exactly what "coordinates" means. Belongs with [[Linear Algebra]].
- $\mathbf{T}$ is a transformation in the sense of [[Matrix Overview]], and $\mathbf{T}^n$ is that transformation composed with itself $n$ times — associativity is what lets $\mathbf{x}_n = \mathbf{T}^n\mathbf{x}_0$ be written at all. Its dilation $D(k, \lambda)$ is an eigen-decomposition already in diagonal form, and its rotation $R(\alpha)$ is the case with no real eigenvector, which is the complex example of §2: rotating matrices have nowhere to stand still.
- Solving for $\alpha, \beta$ and for $c_1, c_2$ by hand is a $2 \times 2$ elimination, and [[Gaussian elimination]] is the same move at $n \times n$. The two ideas meet exactly: vectors are linearly dependent precisely when elimination produces a zero row, so the free variable that appears is the non-trivial $\alpha$ combination this note hunts for. Finding $\mathbf{v}$ once $\lambda$ is known is that algorithm applied to the deliberately singular $(\mathbf{A} - \lambda\mathbf{I})$.
- The step this note assumes is the determinant one: eigenvalues are handed over here ($5$ and $2$, $1$ and $0.7$), but *which* $\lambda$ exist at all is $\det(\mathbf{A} - \lambda\mathbf{I}) = 0$ — the characteristic polynomial of [[Determinants and inverses for matrices]], asking which scalar makes the matrix collapse space.
- The coefficients $c_i$ cost a system of simultaneous equations because an eigenbasis is not orthogonal in general. When it *is* orthonormal, each coordinate is a single projection instead — one dot product per basis vector, as [[Scalar Product]] sets up. That is the whole efficiency argument for orthogonal bases.
- $\mathbf{x}_{n+1} = \mathbf{T}\mathbf{x}_n$ is a recurrence and $\mathbf{x}_n = \mathbf{T}^n\mathbf{x}_0$ its closed form, in the sense of [[basic recursion for sum loop]]. The boxed $\mathbf{A}^k\mathbf{r}_0 = \sum_i c_i\lambda_i^k\mathbf{v}_i$ is the same trade the vault keeps making: pay once to change basis, then the hundredth step is two scalar powers rather than a hundred matrix multiplies, exactly as the closed form in [[Cylinder]] replaces an iteration.
- The continuous twin of §1 is [[population-example]]: a discrete recurrence iterated year by year against a differential equation solved for $P(t)$, with $0.7^k$ decaying to nothing in the same way $e^{-kt}$ does. The $c_i$ fixed by $\mathbf{r}_0$ are the arbitrary constants an initial condition pins down in [[First order differential equations]] — and in [[Second Order differential Equations]] the auxiliary equation *is* this note's eigenvalue equation, with $e^{\lambda t}$ in place of $\lambda^k$ and the trial solution $y = e^{\lambda x}$ working for the same structural reason ([[linear homogenious equations]]). Complex $\lambda$ means rotation here and oscillation there: [[Complex conjugate roots]], read physically in [[dampened harmonic oscillator]].
- Splitting $\mathbf{v}$ into independent eigen-components, evolving each alone and adding them back is superposition — the move that glues a complementary function to a particular integral in [[linear Inhomogernious equations]].
- $\mathbf{T}\mathbf{x} = \mathbf{x}$ is a fixed point, and the vault has a Boolean one: the closure iteration $m_{k+1}(r) = m_k(r)$ in [[Bitset Auth]] stops changing for the same reason the populations do, with $A^* = I \lor A \lor A^2 \lor \cdots$ as matrix powers over OR and AND. What guarantees convergence differs — there it is the topological ordering of the roles, here it is $|\lambda| \le 1$ on everything but the dominant direction.
- The power method at the end is why the graph notes and this one are about the same object: the adjacency matrix that [[kahns algorithm]] walks combinatorially is the matrix whose dominant eigenvector PageRank computes, and a hop in [[Breadth First Search]] is one application of it. Ranking a graph and traversing it are two readings of $\mathbf{A}\mathbf{x}$.
- A 3D rotation has exactly one real eigendirection — its axis, with $\lambda = 1$ — which is the $\hat{n}$ a unit quaternion stores directly: [[quaternions]].
- Part of [[Home]].

## Open

- [ ] Exercise 20: use the expansion to compute $\mathbf{r}_8, \mathbf{r}_9, \mathbf{r}_{10}$ for the Exercise 19 matrix and watch the $5^k$ term swamp the $2^k$ term.
- [ ] Read the PageRank box as a concrete "world's largest eigenvector problem".
- [ ] The characteristic polynomial $\det(\mathbf{A} - \lambda\mathbf{I}) = 0$, to find the eigenvalues this note is handed — the open item [[eigen]] has been carrying.
- [ ] Diagonalisation: $\mathbf{A} = \mathbf{P}\mathbf{D}\mathbf{P}^{-1}$ as §5 written in matrix form, and $\mathbf{A}^k = \mathbf{P}\mathbf{D}^k\mathbf{P}^{-1}$ as the boxed formula with the sum hidden inside the matrices.
- [ ] The defective case properly: $\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}$ from §4, generalised eigenvectors, and what breaks in §5 when the basis is short.
- [ ] The power method as an actual algorithm, normalising each step so the iterate does not overflow or vanish — the practical use of [[Unit vectors]].
- [ ] Why $\lambda = 1$ is guaranteed for the population model: the columns of $\mathbf{T}$ sum to $1$ (nobody leaves the system), which is the Markov/stochastic-matrix condition.
