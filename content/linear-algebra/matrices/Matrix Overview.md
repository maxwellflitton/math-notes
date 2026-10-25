---
tags:
  - maths
  - linear-algebra
  - matrices
  - computer-graphics
created: 2026-09-27
---

# Matrix overview

Part of [[Linear Algebra]]. A matrix is a linear transformation of [[Euclidean space]] written as a grid of numbers: feed it a vector and it hands back the transformed vector.

The order of a matrix is height $\times$ width.

## Matrix multiplication

The product of matrix multiplication can be shown with the following example:

$$
A = \begin{bmatrix} a & b \\ c & d \end{bmatrix} \Rightarrow 2 \times 2
\qquad
x = \begin{bmatrix} x \\ y \end{bmatrix} = 2 \times 1
$$

$$
Ax = (2 \times \underset{\uparrow}{2})(\underset{\uparrow}{2} \times 1) = 2 \times 1
$$

The inner dimensions must match; the outer dimensions give the order of the result.

$$
Ax = \begin{bmatrix} a & b \\ c & d \end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix}
= \begin{bmatrix} ax + by \\ cx + dy \end{bmatrix}
$$

Each entry of the result is a row of $A$ dotted with $x$, so matrix multiplication is a grid of dot products — see [[Scalar Product]]. The general $m \times q$ by $q \times n$ case, with the index formula $c_{ij} = \sum_k a_{ik} b_{kj}$, is worked through in [[Matrix algebra]].

## Composing transformations

Successive transformations can be multiplied together to have one matrix that can be used. A good example of this is a rotation and dilation matrix as seen below:

$$
C = R(\alpha)\,D(k, \lambda)
= \begin{bmatrix} \cos\alpha & -\sin\alpha \\ \sin\alpha & \cos\alpha \end{bmatrix}
\begin{bmatrix} k & 0 \\ 0 & \lambda \end{bmatrix}
= \begin{bmatrix} k\cos\alpha & -\lambda\sin\alpha \\ k\sin\alpha & \lambda\cos\alpha \end{bmatrix}
$$

Other real world examples are projection matrices to convert a 3D object onto a 2D plane. That composition is the whole of a camera: the orthographic case is built factor by factor in [[Ortho derivation]] and assembled into $PV$ in [[Ortho Application]].

We must note here that matrix multiplication is **non-commutative**, so

$$
RD \neq DR
$$

We can also distribute over addition with the following:

$$
A(B + C) = AB + AC
$$

A scalar also follows the following:

$$
\lambda(AB) = A(\lambda B)
$$

Sometimes multiplication *can* be commutative in special cases. For instance, we can show how the following:

$$
R(\alpha)\,R(\beta) = R(\beta)\,R(\alpha) = R(\alpha + \beta)
$$

## Showing $R(\alpha)R(\beta) = R(\alpha + \beta)$

We start with the following:

$$
R(\alpha)\,R(\beta) =
\begin{bmatrix} \cos\alpha & -\sin\alpha \\ \sin\alpha & \cos\alpha \end{bmatrix}
\begin{bmatrix} \cos\beta & -\sin\beta \\ \sin\beta & \cos\beta \end{bmatrix}
$$

This expands to the following:

$$
\begin{bmatrix}
\cos\alpha\cos\beta - \sin\alpha\sin\beta & -\sin\beta\cos\alpha - \sin\alpha\cos\beta \\
\sin\alpha\cos\beta + \cos\alpha\sin\beta & \cos\alpha\cos\beta - \sin\beta\sin\alpha
\end{bmatrix}
$$

We can then use the following expressions from the [[compound angle formula|compound angle formulae]]:

$$
\cos(\alpha + \beta) = \cos\alpha\cos\beta - \sin\alpha\sin\beta
$$

$$
\sin(\alpha + \beta) = \sin\alpha\cos\beta + \cos\alpha\sin\beta
$$

Substituting these in we have the following:

$$
R(\alpha)\,R(\beta) =
\begin{bmatrix}
\cos(\alpha + \beta) & -\sin(\alpha + \beta) \\
\sin(\alpha + \beta) & \cos(\alpha + \beta)
\end{bmatrix}
= R(\alpha + \beta)
$$

## Inversion

With this in mind we can now explore inversion. The inverted matrix does the following:

$$
AA^{-1} = I
$$

With this we can deduce that the dilation matrix has the following inverse:

$$
D(k, \lambda) \text{ is } D^{-1}(k, \lambda) = D\left(\tfrac{1}{k}, \tfrac{1}{\lambda}\right)
$$

and for the rotation:

$$
R^{-1}(\alpha) = R(-\alpha) \quad \text{as} \quad R(\alpha)\,R^{-1}(\alpha) = R(\alpha - \alpha)
$$

### Inverse of a $2 \times 2$ matrix

For a $2 \times 2$ matrix the inverse of the matrix $A$:

$$
A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}
$$

Then the inverse of the matrix is:

$$
A^{-1} = \frac{1}{ad - bc}
\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}
$$

Inversion also follows this pattern:

$$
(ABC)^{-1} = C^{-1}B^{-1}A^{-1}
$$

We also know that we are dividing the matrix by $ad - bc$, so

$$
ad - bc \neq 0
$$

That denominator is the determinant of $A$ — the same $ps - qr$ that measures the area of the parallelogram spanned by the two rows in [[Determinant (cofactor) form of the cross product]]. A determinant of zero means the transformation has flattened the plane onto a line, so there is nothing to invert: information has already been thrown away. The volume version of the same statement is in [[Volumes and the scalar triple product]].

---

## Connections
- Part of [[Linear Algebra]]. A matrix acts on the vectors of [[Basic Operations]] and transforms the space of [[Euclidean space]]; the dot product of [[Scalar Product]] is the single entry that matrix multiplication repeats in a grid. The entry-by-entry index form of that product, for any compatible orders, is in [[Matrix algebra]].
- The determinant $ad - bc$ in the inverse is the signed area of [[Determinant (cofactor) form of the cross product]], and its $3 \times 3$ relative the signed volume of [[Volumes and the scalar triple product]]. "Invertible" and "did not collapse the space" are the same condition.
- $R(\alpha)R(\beta) = R(\alpha + \beta)$ read backwards *is* the rotation-matrix proof of the [[compound angle formula|compound angle formulae]] — there the identities prove the composition, here the composition is assumed and the identities substituted in.
- Composing transformations is what a camera does: [[Ortho derivation]] builds the view and projection matrices from first principles (including $A(BC) = (AB)C$ and why $AB \neq BA$ matters for the order of the factors), and [[Ortho Application]] multiplies them into the single $PV$ the GPU uses. Picking in [[Ortho Application#7. Running it backwards]] is the inversion of this note at $4 \times 4$: $P$ and $V$ are both invertible, so a pixel maps back to one world point, and $\left(PV\right)^{-1} = V^{-1}P^{-1}$ — the same order reversal as $(ABC)^{-1} = C^{-1}B^{-1}A^{-1}$ above.
- Translation is the transformation a $2 \times 2$ or $3 \times 3$ matrix cannot do, because a linear map always fixes the origin. Homogeneous coordinates — tack a $1$ onto the vector and use an extra row — are the workaround, derived in [[Ortho derivation#4. Measure from the eye: the full view matrix]].
- Eigenvectors are the directions a matrix only scales, i.e. the axes hiding inside these transformations — [[eigen]]. The dilation $D(k, \lambda)$ here is already in that form: the axes are $\hat{\imath}, \hat{\jmath}$ with eigenvalues $k$ and $\lambda$, while $R(\alpha)$ has no real eigenvectors at all, which is why rotation and dilation do not commute.
- The entries need not be numbers: over Booleans with OR for addition and AND for multiplication, matrix powers give the transitive closure $A^* = I \lor A \lor A^2 \lor \cdots$ of a permission hierarchy — [[Bitset Auth]].
- Transformations of control points are how curves are moved and scaled in [[CAD Geometry]].
- Where $\frac{1}{ad-bc}\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$ comes from, and the $n \times n$ version of it: the cofactor matrix and adjugate of [[Determinants and inverses for matrices]].
- The same rotations stored as four numbers instead of nine, composed by Hamilton product and inverted by conjugation rather than by $\frac{1}{\det A}C^T$: [[quaternions]], where $R^{-1}(\alpha) = R(-\alpha)$ reappears as $q^{-1} = \bar q$.
- Part of [[Home]].

## Open
- [x] Determinants and inverses beyond $2 \times 2$ — written up in [[Determinants and inverses for matrices]] (Gaussian elimination still open there).
- [ ] Shear and reflection matrices alongside the rotation and dilation above.
- [ ] Diagonalisation: choosing a basis where a matrix is a dilation, following on from [[eigen]].