---
tags:
  - maths
  - linear-algebra
  - matrices
created: 2026-10-03
---

# Matrix algebra

Part of [[Linear Algebra]]. The index notation and dimension rules behind the product that [[Matrix Overview]] uses on $2 \times 2$ matrices, written out for any compatible pair.

We can use matrices to describe systems in multiple dimensions. A simple example would be describing a feeding scheme based on an animal's mass. Each feed amount $m_i$ is a weighted combination of the animal masses:

$$
m_i = a_{i1} M_1 + a_{i2} M_2
$$

We can describe multiple feeds with the following:

$$
\begin{bmatrix} m_1 \\ m_2 \end{bmatrix}
= \begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}
\begin{bmatrix} M_1 \\ M_2 \end{bmatrix}
$$

Row $i$ of the coefficient matrix holds the weights for feed $i$, so the bottom-left entry is $a_{21}$ (row 2, column 1).

With this we can model fairly complex systems by increasing the dimensions. A $20 \times 20$ can give a fairly complex combination of different foods, and matrix algebra can collapse multiple matrices into one. With matrix algebra we must introduce new notation:

$$
A = [a_{ij}]
$$

where $i$ is the row index and $j$ is the column index.

## When multiplication is allowed

Matrix multiplication is allowed if the inner dimensions match. So if $A$ is $m \times q$ and $B$ is $r \times n$, then $AB$ would only work if $q = r$, and the result would be $m \times n$:

$$
AB = (m \times q)(r \times n) = m \times n \qquad (\text{rows} \times \text{columns})
$$

The inner dimensions $q$ and $r$ must agree and then disappear; the outer dimensions $m$ and $n$ are what survive.

## The entry formula

Now let's say that $AB = C$. The inner element result is the following:

$$
c_{ij} = \sum_{k=1}^{r} a_{ik} b_{kj} = a_{i1} b_{1j} + a_{i2} b_{2j} + \dots + a_{ir} b_{rj}
$$

where $r$ is the inner dimension of the multiplication, so for $AB$ this is $r = q$. In words: entry $c_{ij}$ is row $i$ of $A$ dotted with column $j$ of $B$.

## Worked example: $(2 \times 3)(3 \times 2) = (2 \times 2)$

We can show this in action with the following example:

$$
\begin{bmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \end{bmatrix}
\begin{bmatrix} b_{11} & b_{12} \\ b_{21} & b_{22} \\ b_{31} & b_{32} \end{bmatrix}
= \begin{bmatrix} c_{11} & c_{12} \\ c_{21} & c_{22} \end{bmatrix}
$$

The inner dimension is $3$, so each entry of $C$ is a sum of three products:

$$
\begin{aligned}
c_{11} &= \sum_{k=1}^{3} a_{1k} b_{k1} = a_{11} b_{11} + a_{12} b_{21} + a_{13} b_{31} \\[4pt]
c_{12} &= \sum_{k=1}^{3} a_{1k} b_{k2} = a_{11} b_{12} + a_{12} b_{22} + a_{13} b_{32} \\[4pt]
c_{21} &= \sum_{k=1}^{3} a_{2k} b_{k1} = a_{21} b_{11} + a_{22} b_{21} + a_{23} b_{31} \\[4pt]
c_{22} &= \sum_{k=1}^{3} a_{2k} b_{k2} = a_{21} b_{12} + a_{22} b_{22} + a_{23} b_{32}
\end{aligned}
$$

Notice the pattern: the first index of $a$ matches the row of $c$, the second index of $b$ matches the column of $c$, and the index $k$ walks along the row of $A$ and down the column of $B$ together.

## A more concrete example: $(2 \times 1)(1 \times 3) = (2 \times 3)$

$$
\begin{bmatrix} 1 \\ 2 \end{bmatrix}
\begin{bmatrix} 3 & 0 & -4 \end{bmatrix}
$$

The inner dimension is $1$, so each entry is a single product. The result is:

$$
\begin{bmatrix} 3 & 0 & -4 \\ 6 & 0 & -8 \end{bmatrix}
$$

This is an **outer product**: a column times a row gives a full matrix, where every row is a scaled copy of $\begin{bmatrix} 3 & 0 & -4 \end{bmatrix}$. The reverse order, $\begin{bmatrix} 3 & 0 & -4 \end{bmatrix} \begin{bmatrix} 1 \\ 2 \end{bmatrix}$, doesn't work at all because the inner dimensions ($3$ and $2$) don't match. Matrix multiplication is not commutative, and the dimension rule is the first place that shows up.

---

## Connections

- Part of [[Linear Algebra]]. [[Matrix Overview]] applies this product to $2 \times 2$ rotations and dilations, composes them, and inverts them; this note is the general $m \times q$ by $q \times n$ case written entry by entry.
- Each $c_{ij} = \sum_k a_{ik} b_{kj}$ is row $i$ of $A$ dotted with column $j$ of $B$, so matrix multiplication is a grid of dot products — [[Scalar Product]] repeated once per cell of $C$.
- The square case has a determinant, and it multiplies: $\det(AB) = \det A \det B$, with $A^{-1} = \frac{1}{\det A} C^{T}$ when it is non-zero — [[Determinants and inverses for matrices]].
- The outer product above is the blunt case of non-commutativity: $AB$ exists while $BA$ does not, because the inner dimensions stop matching when you swap them. The $2 \times 2$ case where both products exist but differ, $RD \neq DR$, is in [[Matrix Overview]].
- The matrices act on the component-form vectors of [[Basic Operations]], transforming the space of [[Euclidean space]].
- Collapsing several matrices into one is what a camera does: [[Ortho derivation]] builds the factors and [[Ortho Application]] multiplies them into a single $PV$.
- Replace $\times$ with AND and $+$ with OR and the same $c_{ij}$ formula computes reachability in a permission graph — [[Bitset Auth]].
- Reading the product as a system instead of a transformation: $A\mathbf{x} = \mathbf{b}$ is one equation per row, solved by row operations in [[Gaussian elimination]].
- Part of [[Home]].
