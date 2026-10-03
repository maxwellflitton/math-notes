# Transposing a matrix

We can transpose a matrix with the following, swapping rows for columns:

$$
\begin{bmatrix} a & b \\ c & d \end{bmatrix}^{T}
= \begin{bmatrix} a & c \\ b & d \end{bmatrix}
$$

For example:

$$
\begin{bmatrix} 2 & 7 \\ -6 & 1 \\ 0 & 4 \end{bmatrix}^{T}
= \begin{bmatrix} 2 & -6 & 0 \\ 7 & 1 & 4 \end{bmatrix}
$$

A $3 \times 2$ becomes a $2 \times 3$: the first column $(2, -6, 0)$ becomes the first row, and so on. The main diagonal stays where it is; everything else is reflected across it.

## Definition

$$
A \Rightarrow A^{T} \qquad [a_{ij}]^{T} = [a_{ji}]
$$

We note that $i$ and $j$ are reversed: the entry in row $i$, column $j$ of $A^{T}$ is the entry in row $j$, column $i$ of $A$.

## Transpose of a product

We also note that

$$
(AX)^{T} = X^{T} A^{T}
$$

The order reverses. This has to happen for the dimensions to line up: if $A$ is $m \times q$ and $X$ is $q \times n$, then $(AX)^{T}$ is $n \times m$, and the only way to multiply the transposes to get $n \times m$ is $X^{T}$ ($n \times q$) times $A^{T}$ ($q \times m$). See [[matrix-algebra]] for the dimension rule.

Other useful facts:

- $(A^{T})^{T} = A$
- $(A + B)^{T} = A^{T} + B^{T}$
- $\det(A^{T}) = \det A$, so expanding a determinant along a column is the same as along a row
- A matrix with $A^{T} = A$ is called **symmetric**

## Links

- [[matrix-algebra]]: dimension rule and the entry formula
- [[determinants-and-inverses]]: $A^{-1} = \frac{1}{\det A} C^{T}$ uses the transpose of the cofactor matrix