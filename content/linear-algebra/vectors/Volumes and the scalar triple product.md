Follows on from [[Determinant (cofactor) form of the cross product|the determinant form of the cross product]]. Exercise 19 used two vectors to get an **area**. This section adds a third vector to get a **volume**.

---

## The volume of a parallelepiped

A parallelepiped is a distorted brick: six faces, all parallelograms. Three vectors $\vec{a}$, $\vec{b}$, $\vec{c}$ from one corner define its three edges. Its volume is

$$V = \text{base area} \times \text{vertical height} = \lvert (\vec{a} \times \vec{b}) \cdot \vec{c} \rvert$$

The recipe is two operations I already have, one after the other:

1. **Cross product** $\vec{a} \times \vec{b}$. Its magnitude is the area of the base parallelogram. Its direction is perpendicular to the base.
2. **Dot product** with $\vec{c}$. Dotting $\vec{c}$ against a vector perpendicular to the base picks out only the part of $\vec{c}$ that points straight up off the base, which is the height $h$.

So the cross gives the area and points "up", and the dot measures how far $\vec{c}$ goes in that "up" direction. Area times height is the volume.

$(\vec{a} \times \vec{b}) \cdot \vec{c}$ is called the **scalar triple product**: three vectors in, one number out.

> [!note] Why it is not a special case of the cross product
> The cross product on its own always gives an area, whatever dimension you are in. Two vectors can only bound a flat shape. A volume needs a third vector, and the triple product is a new operation built from a cross and a dot together. It is the count of vectors that matters, not the dimension.

> [!note] The absolute value
> $(\vec{a} \times \vec{b}) \cdot \vec{c}$ can come out negative, if $\vec{c}$ points to the opposite side of the base from $\vec{a} \times \vec{b}$. The sign tells you the **orientation** (handedness) of the three vectors. The bars throw the sign away when all you want is the volume.

---

## Worked example 1: the skewed brick

![Skewed parallelepiped: a and b span the shaded base, c leans forward and to the right, h is the perpendicular drop from the tip of c to the base](_attachments/triple-product-skewed.svg)

*Figure 1. The same shape as the textbook's Figure 29: $\vec{a}$ and $\vec{b}$ lay out the shaded base, $\vec{c}$ leans forward and to the right, and $h$ is the perpendicular drop from the tip of $\vec{c}$ to the base.*

Take

$$\vec{a} = (3, 0, 0) \qquad \vec{b} = (1, 2, 0) \qquad \vec{c} = (1, 1, 2)$$

**Step 1, the base.** Cross product in cofactor form, exactly as in [[Determinant (cofactor) form of the cross product]]:

$$\vec{a} \times \vec{b} = \begin{vmatrix} \hat{\imath} & \hat{\jmath} & \hat{k} \\ 3 & 0 & 0 \\ 1 & 2 & 0 \end{vmatrix}$$

$$\hat{\imath}: \begin{vmatrix} 0 & 0 \\ 2 & 0 \end{vmatrix} = 0 \qquad \hat{\jmath}: -\begin{vmatrix} 3 & 0 \\ 1 & 0 \end{vmatrix} = 0 \qquad \hat{k}: \begin{vmatrix} 3 & 0 \\ 1 & 2 \end{vmatrix} = (3)(2) - (0)(1) = 6$$

$$\vec{a} \times \vec{b} = (0, 0, 6)$$

The base area is $\lvert \vec{a} \times \vec{b} \rvert = 6$, and the vector points straight up the $z$-axis, because the base lies flat in the $xy$-plane.

**Step 2, the height.** Dot with $\vec{c}$:

$$(\vec{a} \times \vec{b}) \cdot \vec{c} = (0)(1) + (0)(1) + (6)(2) = 12$$

$$V = 12$$

**Check it by hand.** Base area 6. The height is only the $z$-component of $\vec{c}$, which is 2, because the base is horizontal. The $x$ and $y$ parts of $\vec{c}$ just slide the top face sideways (that is the lean in Figure 1), and sliding a face sideways does not change the volume.

$$6 \times 2 = 12 \quad \checkmark$$

This is the whole point of the dot product here. $\vec{c}$ has length $\sqrt{1 + 1 + 4} = \sqrt{6} \approx 2.45$, but only $2$ of that is height. The dot product with a perpendicular vector strips the lean away automatically.

---

## Worked example 2: the cube

![Cube: a, b and c mutually perpendicular, so c is itself the height above the base](_attachments/triple-product-cube.svg)

*Figure 2. When the three vectors are mutually perpendicular the brick is a cube, and $\vec{c}$ is the height with nothing to strip away.*

Take

$$\vec{a} = (2, 0, 0) \qquad \vec{b} = (0, 2, 0) \qquad \vec{c} = (0, 0, 2)$$

**Step 1, the base.**

$$\vec{a} \times \vec{b} = \begin{vmatrix} \hat{\imath} & \hat{\jmath} & \hat{k} \\ 2 & 0 & 0 \\ 0 & 2 & 0 \end{vmatrix} = (0, 0, 4)$$

Base area $4$, which is $2 \times 2$ as expected for a square.

**Step 2, the height.**

$$(\vec{a} \times \vec{b}) \cdot \vec{c} = (0)(0) + (0)(0) + (4)(2) = 8$$

$$V = 8 = 2^3 \quad \checkmark$$

The cube is not an exception to the formula, it is the easiest case of it. $\vec{c}$ is already perpendicular to the base, so the height is the full length of $\vec{c}$ and the dot product has nothing to throw away. Compare Figure 1, where the same dot product silently discarded the sideways part of $\vec{c}$.

> [!note] Coplanar test
> If $\vec{c}$ lay flat in the base plane, say $\vec{c} = (1, 1, 0)$ in example 1, then $(0, 0, 6) \cdot (1, 1, 0) = 0$. Zero volume means the three vectors are **coplanar**. That is a useful test in its own right.

---

## The cyclic identity

The textbook's equation (36):

$$\vec{a} \cdot (\vec{b} \times \vec{c}) = \vec{b} \cdot (\vec{c} \times \vec{a}) = \vec{c} \cdot (\vec{a} \times \vec{b})$$

Geometrically this says: it does not matter which pair of edges you call the base. Same brick, same volume. With example 1:

$$\vec{b} \cdot (\vec{c} \times \vec{a}) = (1, 2, 0) \cdot (0, 6, -3) = 0 + 12 + 0 = 12 \quad \checkmark$$

Equation (37) then follows because the dot product commutes:

$$\vec{a} \cdot (\vec{b} \times \vec{c}) = (\vec{a} \times \vec{b}) \cdot \vec{c}$$

The cross and the dot can swap places, as long as the three letters stay in the same cyclic order $a \to b \to c \to a$. Swapping two of them, e.g. $(\vec{b} \times \vec{a}) \cdot \vec{c}$, flips the sign to $-12$ (the cross product is anti-commutative). Same volume, opposite orientation.

---

## Where this is leading

This section is the last of the purely geometric build-up before the book moves to matrices and linear transformations, and it is the bridge between the two.

**The triple product is a $3 \times 3$ determinant.** Stack the three vectors as rows:

$$(\vec{a} \times \vec{b}) \cdot \vec{c} = \begin{vmatrix} a_1 & a_2 & a_3 \\ b_1 & b_2 & b_3 \\ c_1 & c_2 & c_3 \end{vmatrix}$$

For example 1:

$$\begin{vmatrix} 3 & 0 & 0 \\ 1 & 2 & 0 \\ 1 & 1 & 2 \end{vmatrix} = 3 \begin{vmatrix} 2 & 0 \\ 1 & 2 \end{vmatrix} - 0 + 0 = 3 \times 4 = 12 \quad \checkmark$$

So the pattern from the last two notes completes:

| Vectors | Determinant | Measures |
|---|---|---|
| 2 in 2D | $2 \times 2$ | signed area |
| 3 in 3D | $3 \times 3$ | signed volume |

The cofactor expansion I used for the cross product was really a $3 \times 3$ determinant with unit vectors in the top row. Replace that row with a real third vector and you get the volume directly, in one pass.

**Why this matters for matrices.** A $3 \times 3$ matrix is a linear transformation of space. Its three rows (or columns) are where the three basis vectors land after the transformation. The determinant is the volume of the brick they span, i.e. the **volume scaling factor** of the transformation:

- $\det = 1$: volume preserved (rotations)
- $\det = 2$: every volume doubles
- $\det = 0$: space is squashed flat, the transformation is not invertible
- $\det < 0$: the orientation is flipped (a reflection is involved)

That is the reading to carry into the next section on linear transformations. The determinant is not an arbitrary formula, it is "how much does this matrix scale volume, and does it flip handedness". Both the area case (Exercise 19) and the volume case (this section) are the same idea one dimension apart.

---

## Symbols

| Symbol | Meaning |
|---|---|
| $(\vec{a} \times \vec{b}) \cdot \vec{c}$ | scalar triple product: cross first, then dot |
| $V$ | volume of the parallelepiped |
| $h$ | perpendicular height, the component of $\vec{c}$ along $\vec{a} \times \vec{b}$ |
| $\lvert \cdot \rvert$ around a scalar | absolute value, discards the orientation sign |
| $\lvert \vec{v} \rvert$ | magnitude of a vector |
| $\begin{vmatrix} \cdot \end{vmatrix}$ | determinant |
| coplanar | three vectors lying in one plane, triple product zero |
| cyclic order | $a \to b \to c \to a$; rotating the letters keeps the sign, swapping two flips it |

---

## Connections
- Part of [[Linear Algebra]]. This is the two operations of [[Vector Product]] and [[Scalar Product]] run back to back: the cross supplies the base area and the "up" direction, the dot measures how far the third vector travels in that direction.
- The cofactor expansion of [[Determinant (cofactor) form of the cross product]] is this same $3 \times 3$ determinant with $\hat{\imath}, \hat{\jmath}, \hat{k}$ in the top row. Replace that row with a real third vector and the mnemonic becomes an honest determinant returning a signed volume.
- The coplanar test $(\vec{a} \times \vec{b}) \cdot \vec{c} = 0$ is the three-vector version of the orthogonality test of [[Scalar Product]] and the parallel test of [[Vector Product]] — all three are the same statement that a spanned shape has collapsed.
- Stripping the lean off $\vec{c}$ is projection onto $\widehat{\vec{a} \times \vec{b}}$, the projection $\hat{u}\cdot\vec{a}$ of [[Scalar Product]] applied to a normal instead of an edge, with heights and volumes in [[Euclidean space]] where areas and lengths were before.
- $\det = 0$ meaning "space squashed flat, not invertible" is the same degeneracy $\det(A - \lambda I) = 0$ exploits to find eigenvalues — the still-open characteristic polynomial of [[eigen]].
- A renderer uses the sign rather than the size: the triple product of three edge vectors says which side of a face a point falls on, the orientation test behind winding order and inside/outside checks on the index triples of [[Cylinder]] and [[Sphere]] — [[CAD Geometry]].
- Part of [[Home]].
