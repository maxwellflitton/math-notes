---
tags:
  - maths
  - linear-algebra
  - matrices
created: 2026-10-03
---

# Determinants and inverses for matrices

Part of [[Linear Algebra]], and the general case of the $2 \times 2$ inverse in [[Matrix Overview]]. A square matrix is a transformation of space, and the determinant is the one number that tells you how much that transformation stretches or squashes the size of things. The sign of the determinant tells you whether the transformation flips orientation (a negative determinant includes a reflection), and a determinant of zero means something got flattened. The specific cases fall out of this.

## The $2 \times 2$ case: area

For a $2 \times 2$ matrix we have the following:

$$
A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}, \qquad
\det A = \begin{vmatrix} a & b \\ c & d \end{vmatrix} = ad - bc
$$

This gives the area. The two vectors are the columns of the matrix, drawn from the origin:

$$
\mathbf{i} \mapsto \begin{bmatrix} a \\ c \end{bmatrix} \qquad
\mathbf{j} \mapsto \begin{bmatrix} b \\ d \end{bmatrix}
$$

These are where the unit vectors land after the transformation (multiplying $A$ by $(1, 0)$ returns the first column). The two vectors from the origin are adjacent sides of a **parallelogram** whose fourth corner is at $(a + b,\ c + d)$, and the determinant is the area of that parallelogram. (The triangle made by the two vectors has half that area, $\tfrac{1}{2}|ad - bc|$.)

If the two vectors point the same way the parallelogram is flat, the area is zero, and the matrix is not invertible.

## The $3 \times 3$ case: volume

For higher dimensions we use recursion. For a $3 \times 3$ we have the following:

$$
\det \begin{bmatrix} a_1 & a_2 & a_3 \\ b_1 & b_2 & b_3 \\ c_1 & c_2 & c_3 \end{bmatrix}
= a_1 \begin{vmatrix} b_2 & b_3 \\ c_2 & c_3 \end{vmatrix}
- a_2 \begin{vmatrix} b_1 & b_3 \\ c_1 & c_3 \end{vmatrix}
+ a_3 \begin{vmatrix} b_1 & b_2 \\ c_1 & c_2 \end{vmatrix}
$$

Each entry in the top row is multiplied by the determinant of the $2 \times 2$ left over when you cross out that entry's row and column, with alternating signs $+,\ -,\ +$. Then we calculate the determinants of the three $2 \times 2$ matrices. The result is still a single number: it is recursion, not dimension reduction.

For a $3 \times 3$ the determinant is volume. The three rows (or columns) are three vectors from the origin, and the determinant is the volume of the parallelepiped (slanted box) they span.

## The cross product

If $\mathbf{b} = b_x \hat{\mathbf{i}} + b_y \hat{\mathbf{j}} + b_z \hat{\mathbf{k}}$ and $\mathbf{c} = c_x \hat{\mathbf{i}} + c_y \hat{\mathbf{j}} + c_z \hat{\mathbf{k}}$ are two vectors, we can denote the cross product with the following:

$$
\mathbf{b} \times \mathbf{c}
= \begin{vmatrix} \hat{\mathbf{i}} & \hat{\mathbf{j}} & \hat{\mathbf{k}} \\ b_x & b_y & b_z \\ c_x & c_y & c_z \end{vmatrix}
= \begin{vmatrix} b_y & b_z \\ c_y & c_z \end{vmatrix} \hat{\mathbf{i}}
- \begin{vmatrix} b_x & b_z \\ c_x & c_z \end{vmatrix} \hat{\mathbf{j}}
+ \begin{vmatrix} b_x & b_y \\ c_x & c_y \end{vmatrix} \hat{\mathbf{k}}
$$

This is the same top-row expansion pattern as the $3 \times 3$ determinant, borrowed by the cross product. It is a bit of a cheat formally (mixing vectors and numbers in one matrix) but it works because the expansion is just a linear combination of the top row.

The length $|\mathbf{b} \times \mathbf{c}|$ is the area of the parallelogram spanned by $\mathbf{b}$ and $\mathbf{c}$, and the direction is perpendicular to that parallelogram. The three $2 \times 2$ determinants are the signed areas of the parallelogram's shadows on the three coordinate planes.

## The scalar triple product: volume

As the cross product gives the area, taking the dot product of the cross product with a third vector gives volume (base area times height). This is done by the following:

$$
\mathbf{a} \cdot (\mathbf{b} \times \mathbf{c})
= \begin{vmatrix} a_x & a_y & a_z \\ b_x & b_y & b_z \\ c_x & c_y & c_z \end{vmatrix}
= a_x \begin{vmatrix} b_y & b_z \\ c_y & c_z \end{vmatrix}
- a_y \begin{vmatrix} b_x & b_z \\ c_x & c_z \end{vmatrix}
+ a_z \begin{vmatrix} b_x & b_y \\ c_x & c_y \end{vmatrix}
$$

The dot product _is_ the determinant: two routes to the same number. Expanding along the top row is the dot product of the top row with the cofactors, and the cofactors of the top row are exactly the components of $\mathbf{b} \times \mathbf{c}$.

So the way to remember it is by counting vectors, not matrix size. Two vectors give area (via the $2 \times 2$ determinant or the cross product). Three vectors give volume (via the $3 \times 3$ determinant or the triple product).

### Worked example: area from the cross product

$$
\mathbf{r}_1 = (1, 1, 0), \qquad \mathbf{r}_2 = (2, 2, -2), \qquad
\text{area} = |\mathbf{r}_1 \times \mathbf{r}_2|
$$

$$
\mathbf{r}_1 \times \mathbf{r}_2
= \begin{vmatrix} \hat{\mathbf{i}} & \hat{\mathbf{j}} & \hat{\mathbf{k}} \\ 1 & 1 & 0 \\ 2 & 2 & -2 \end{vmatrix}
= \hat{\mathbf{i}} \begin{vmatrix} 1 & 0 \\ 2 & -2 \end{vmatrix}
- \hat{\mathbf{j}} \begin{vmatrix} 1 & 0 \\ 2 & -2 \end{vmatrix}
+ \hat{\mathbf{k}} \begin{vmatrix} 1 & 1 \\ 2 & 2 \end{vmatrix}
$$

$$
\mathbf{r}_1 \times \mathbf{r}_2
= \hat{\mathbf{i}}(-2) - \hat{\mathbf{j}}(-2) + \hat{\mathbf{k}}(2 - 2)
= (-2,\ 2,\ 0)
$$

$$
\text{area} = \sqrt{(-2)^2 + 2^2 + 0^2} = \sqrt{8} = 2\sqrt{2}
$$

With this we can conclude that the volume spanned by three vectors is $|\mathbf{a} \cdot (\mathbf{b} \times \mathbf{c})|$.

## The inverse

Finally, the inverse of a matrix is a combination of the determinant and the transpose of the cofactor matrix:

$$
A^{-1} = \frac{1}{\det A}\,C^{T}
$$

where $C$ is the **cofactor matrix**: each entry $C_{ij}$ is the determinant of the $2 \times 2$ you get by crossing out row $i$ and column $j$, with the sign from the checkerboard pattern

$$
\begin{bmatrix} + & - & + \\ - & + & - \\ + & - & + \end{bmatrix}
$$

$C^{T}$ is called the adjugate. This only works when $\det A \neq 0$, which ties back to the geometry: you cannot undo a squash.

### The $2 \times 2$ case

The cofactor matrix is essentially $A$ "flipped":

$$
A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}
\;\Rightarrow\;
C = \begin{bmatrix} d & -c \\ -b & a \end{bmatrix}
\;\Rightarrow\;
C^{T} = \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}
$$

$$
A^{-1} = \frac{1}{ad - bc} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}
$$

### The $3 \times 3$ case

For a $3 \times 3$ matrix we calculate $C$ entry by entry:

$$
A = \begin{bmatrix} 2 & 2 & -1 \\ 3 & 5 & 1 \\ 1 & 2 & 1 \end{bmatrix}
$$

$$
\begin{aligned}
C_{11} &= +\begin{vmatrix} 5 & 1 \\ 2 & 1 \end{vmatrix} = 3
& C_{12} &= -\begin{vmatrix} 3 & 1 \\ 1 & 1 \end{vmatrix} = -2
& C_{13} &= +\begin{vmatrix} 3 & 5 \\ 1 & 2 \end{vmatrix} = 1 \\[6pt]
C_{21} &= -\begin{vmatrix} 2 & -1 \\ 2 & 1 \end{vmatrix} = -4
& C_{22} &= +\begin{vmatrix} 2 & -1 \\ 1 & 1 \end{vmatrix} = 3
& C_{23} &= -\begin{vmatrix} 2 & 2 \\ 1 & 2 \end{vmatrix} = -2 \\[6pt]
C_{31} &= +\begin{vmatrix} 2 & -1 \\ 5 & 1 \end{vmatrix} = 7
& C_{32} &= -\begin{vmatrix} 2 & -1 \\ 3 & 1 \end{vmatrix} = -5
& C_{33} &= +\begin{vmatrix} 2 & 2 \\ 3 & 5 \end{vmatrix} = 4
\end{aligned}
$$

So the value of $C$ is the following:

$$
C = \begin{bmatrix} 3 & -2 & 1 \\ -4 & 3 & -2 \\ 7 & -5 & 4 \end{bmatrix}
$$

The determinant comes for free from the first row of $A$ dotted with the first row of $C$:

$$
\det A = 2(3) + 2(-2) + (-1)(1) = 1
$$

Since $\det A = 1$, the inverse is just the transpose of the cofactor matrix:

$$
A^{-1} = \frac{1}{1} C^{T} = \begin{bmatrix} 3 & -4 & 7 \\ -2 & 3 & -5 \\ 1 & -2 & 4 \end{bmatrix}
$$

Check: $A A^{-1} = I$ (first row of $A$ against first column of $A^{-1}$ gives $6 - 4 - 1 = 1$, against the second column gives $-8 + 6 + 2 = 0$, and so on).

## Why the determinant is _the_ number

The determinant respects matrix multiplication, $\det(AB) = \det(A)\det(B)$: doing one transformation then another scales volume by the product of the two scalings. From that follow $\det(A^{-1}) = 1 / \det A$, invertibility exactly when $\det A \neq 0$, eigenvalues from $\det(A - \lambda I) = 0$, and the Jacobian determinant as the local volume scaling in a change of variables. It is also the _only_ function of a square matrix that is linear in each column, flips sign when two columns are swapped, and gives $1$ for the identity. So it is forced by what "signed volume" has to mean, not merely a convenient choice.

---

## Connections

- Part of [[Linear Algebra]]. This completes [[Matrix Overview]]: that note inverts a $2 \times 2$ with $\frac{1}{ad-bc}\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$ and stops there, and the adjugate formula here is where that comes from — $C^T$ for a $2 \times 2$ is just $A$ with the diagonal swapped and the off-diagonal negated.
- The $2 \times 2$ determinant as signed area is derived in [[Determinant (cofactor) form of the cross product]], where the three minors of the $\hat{\imath}\,\hat{\jmath}\,\hat{k}$ expansion are read as shadow areas on the coordinate planes; the $3 \times 3$ as signed volume is [[Volumes and the scalar triple product]], including the cyclic identity and the coplanar test $\det = 0$.
- "The dot product _is_ the determinant" is the geometric definition of [[Vector Product]] meeting the coordinate recipe: the cofactors of the top row are the components of $\mathbf{b} \times \mathbf{c}$, so the triple product is a single [[Scalar Product]] against them.
- $\det A \neq 0$ is the invertibility condition, and at $4 \times 4$ it is what makes picking possible: [[Ortho Application#7. Running it backwards]] inverts $PV$ to take a pixel back to a world ray, with $(PV)^{-1} = V^{-1}P^{-1}$ — the order reversal of [[Matrix Overview]].
- $\det(A - \lambda I) = 0$ is the characteristic polynomial, i.e. "which $\lambda$ makes this matrix collapse the space" — the still-open machinery of [[eigen]]. A determinant of zero and a non-trivial null space are the same fact.
- Cofactor expansion is recursion with the $2 \times 2$ as its base case, in the sense of [[basic recursion for sum loop]]. It is also a worked example of a correct but expensive algorithm: $n!$ terms for an $n \times n$ matrix against $O(n^3)$ for elimination, the same correctness-versus-cost split as exhaustive search in [[introduction to algorithms]].
- The transformations being inverted act on the vectors of [[Basic Operations]] in [[Euclidean space]], and composing them entry by entry is [[Matrix algebra]].
- $\det(AB) = \det A \det B$ is why a chain of model, view and projection matrices has one overall volume scaling — the camera of [[Ortho derivation]], and the control-point transforms of [[CAD Geometry]].
- The $n!$ cost of cofactor expansion is why nothing computes a determinant this way past $3 \times 3$: triangularising with row operations gives the same number as a product of pivots in $O(n^3)$, and the same sweep solves $A\mathbf{x} = \mathbf{b}$ — [[Gaussian elimination]].
- Part of [[Home]].

## Open

- [x] Gaussian elimination as the practical determinant and inverse — [[Gaussian elimination]]; LU, pivoting and the determinant as a product of pivots are still open there.
- [ ] Shear and reflection matrices, and reading $\det < 0$ off them directly.
- [ ] Diagonalisation and the characteristic polynomial, following on from [[eigen]].
