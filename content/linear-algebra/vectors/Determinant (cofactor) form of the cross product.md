# Determinant (cofactor) form of the cross product

We can show this with the cross product of $\vec{b}$ and $\vec{c}$:

$$\vec{b} \times \vec{c} = \begin{vmatrix} \hat{\imath} & \hat{\jmath} & \hat{k} \\ b_1 & b_2 & b_3 \\ c_1 & c_2 & c_3 \end{vmatrix}$$

> [!note] Notation
> Vertical bars mean "the determinant of". Square brackets mean the matrix itself. A matrix is a grid of numbers, a determinant is the single number you get out of it.
>
> This $3 \times 3$ is a mnemonic rather than a true determinant, since the top row holds unit vectors and not numbers. It works because expanding along that row gives exactly the right components.

---

## Expanding it

We expand along the top row:

$$\vec{b} \times \vec{c} = \hat{\imath} \begin{vmatrix} b_2 & b_3 \\ c_2 & c_3 \end{vmatrix} - \hat{\jmath} \begin{vmatrix} b_1 & b_3 \\ c_1 & c_3 \end{vmatrix} + \hat{k} \begin{vmatrix} b_1 & b_2 \\ c_1 & c_2 \end{vmatrix}$$

For each component, cover that component's column (and the top row). The $2 \times 2$ left over is its determinant, called the **minor**. The signs alternate $+\ -\ +$, which is why the $\hat{\jmath}$ term is subtracted.

The $2 \times 2$ determinant is:

$$\begin{vmatrix} p & q \\ r & s \end{vmatrix} = ps - qr$$

Written out in full, the expansion gives:

$$\vec{b} \times \vec{c} = (b_2 c_3 - b_3 c_2,\ \ b_3 c_1 - b_1 c_3,\ \ b_1 c_2 - b_2 c_1)$$

---

## What the determinant means

Geometrically, a $2 \times 2$ determinant is the **signed area of the parallelogram** spanned by its two rows. If the two rows point the same way the area collapses to zero. Swapping the rows flips the sign.

The determinant is complementary to the dot product. The dot product gives us **alignment**, whereas the determinant gives us **spread**. The dot product has a cosine relationship with the angle, whereas the determinant has a sine relationship with the angle.

For two vectors $\vec{u} = (p, q)$ and $\vec{v} = (r, s)$ in 2D:

$$\vec{u} \cdot \vec{v} = pr + qs = \lvert\vec{u}\rvert \lvert\vec{v}\rvert \cos\theta$$

$$\begin{vmatrix} p & q \\ r & s \end{vmatrix} = ps - qr = \lvert\vec{u}\rvert \lvert\vec{v}\rvert \sin\theta$$

| | Dot product | Determinant |
|---|---|---|
| Measures | alignment | spread (area) |
| Trig part | $\cos\theta$ | $\sin\theta$ |
| Vectors parallel | maximum | zero |
| Vectors perpendicular | zero | maximum |

The magnitudes factor out the same way in both, and only the trig part differs. Together they pin down the angle completely, which is how `atan2(det, dot)` recovers a signed angle between two vectors.

> [!note] On the word "orthogonal"
> My first phrasing was "the determinant is orthogonal to the dot product". That is fine in the loose sense of "independent, complementary", but orthogonal already has a precise meaning (perpendicular), so "complementary" is the safer word.

In 3D the same sine relationship holds for the whole cross product:

$$\lvert \vec{b} \times \vec{c} \rvert = \lvert\vec{b}\rvert \lvert\vec{c}\rvert \sin\theta$$

Each individual $2 \times 2$ minor is the area of the **shadow** the parallelogram casts on one coordinate plane: the $\hat{\imath}$ component is the shadow on the $yz$-plane, $\hat{\jmath}$ on the $xz$-plane, $\hat{k}$ on the $xy$-plane.

---

## Worked example

With this in mind we have $\vec{b} = (2, 1, 1)$ and $\vec{c} = (1, -1, -1)$:

$$\vec{b} \times \vec{c} = \begin{vmatrix} \hat{\imath} & \hat{\jmath} & \hat{k} \\ 2 & 1 & 1 \\ 1 & -1 & -1 \end{vmatrix}$$

**The $\hat{\imath}$ component** (cover the $\hat{\imath}$ column):

$$\begin{vmatrix} 1 & 1 \\ -1 & -1 \end{vmatrix} = (1)(-1) - (1)(-1) = -1 + 1 = 0$$

**The $\hat{\jmath}$ component** (cover the $\hat{\jmath}$ column, and remember the minus sign):

$$-\begin{vmatrix} 2 & 1 \\ 1 & -1 \end{vmatrix} = -\big[(2)(-1) - (1)(1)\big] = -(-3) = 3$$

**The $\hat{k}$ component** (cover the $\hat{k}$ column):

$$\begin{vmatrix} 2 & 1 \\ 1 & -1 \end{vmatrix} = (2)(-1) - (1)(1) = -3$$

So:

$$\vec{b} \times \vec{c} = (0,\ 3,\ -3)$$

> [!note] Why the $\hat{\imath}$ component is zero
> Dropping the $x$ coordinate projects $\vec{b}$ to $(1, 1)$ and $\vec{c}$ to $(-1, -1)$ in the $yz$-plane. Those two point in exactly opposite directions, so the shadow parallelogram on that plane is flat and has zero area.

---

## Magnitude

We take the magnitude:

$$\lvert \vec{b} \times \vec{c} \rvert = \sqrt{0^2 + 3^2 + (-3)^2} = \sqrt{18} = 3\sqrt{2}$$

This is the area of the parallelogram spanned by $\vec{b}$ and $\vec{c}$. The triangle with corners at the origin, $\vec{b}$ and $\vec{c}$ is half of that:

$$\text{triangle area} = \tfrac{1}{2} \lvert \vec{b} \times \vec{c} \rvert = \frac{3\sqrt{2}}{2}$$

---

## Checks

**The result is perpendicular to both inputs.** Its dot product with each should be zero:

$$(0, 3, -3) \cdot (2, 1, 1) = 0 + 3 - 3 = 0 \quad \checkmark$$

$$(0, 3, -3) \cdot (1, -1, -1) = 0 - 3 + 3 = 0 \quad \checkmark$$

**The alignment / spread picture holds.** These two vectors happen to be perpendicular:

$$\vec{b} \cdot \vec{c} = (2)(1) + (1)(-1) + (1)(-1) = 0$$

So alignment is zero, $\cos\theta = 0$, and $\sin\theta = 1$. Spread should then be at its maximum, the plain product of the two lengths:

$$\lvert\vec{b}\rvert = \sqrt{4 + 1 + 1} = \sqrt{6} \qquad \lvert\vec{c}\rvert = \sqrt{1 + 1 + 1} = \sqrt{3}$$

$$\lvert\vec{b}\rvert \lvert\vec{c}\rvert \sin\theta = \sqrt{6} \cdot \sqrt{3} \cdot 1 = \sqrt{18} = 3\sqrt{2} \quad \checkmark$$

which matches the magnitude above. Dot product zero, cross product maximal: the two are complementary.

---

## Symbols

| Symbol | Meaning |
|---|---|
| $\vec{b} \times \vec{c}$ | cross product, a vector perpendicular to both |
| $\vec{b} \cdot \vec{c}$ | dot product, a number |
| $\begin{vmatrix} \cdot \end{vmatrix}$ | determinant of the grid inside |
| $\hat{\imath}, \hat{\jmath}, \hat{k}$ | unit vectors along $x$, $y$, $z$ |
| minor | the $2 \times 2$ determinant left after covering a row and column |
| cofactor | the minor with its $+$ or $-$ sign attached |
| $\lvert\vec{v}\rvert$ | magnitude (length) of $\vec{v}$ |
| $\theta$ | angle between the two vectors |

---

## Connections
- Part of [[Linear Algebra]]. This is the coordinate recipe for the [[Vector Product]], whose definition $(|\vec{b}||\vec{c}|\sin\theta)\,\hat{n}$ is geometric and says nothing about how to compute it; the Checks section verifies both halves of that definition — perpendicular to both inputs, magnitude $|\vec{b}||\vec{c}|\sin\theta$.
- The alignment/spread table is the contrast table from [[Vector Product]] with the algebra behind it. The perpendicularity checks are the orthogonality test of [[Scalar Product]], and that note derives $a_xb_x + a_yb_y + a_zb_z$ the same way this one derives the minors: expand by bilinearity and watch the cross-terms vanish.
- `atan2(det, dot)` — sine part against cosine part to recover a signed angle — is exactly Step 6 of the $R$-formula in [[compound angle formula]], where $\theta = \operatorname{atan2}(C, D)$ fixes the quadrant that $\arctan$ alone cannot.
- Each minor as a *shadow* is projection onto a coordinate plane, the plane version of the projection $\hat{u}\cdot\vec{a}$ in [[Scalar Product]]. Squaring and adding the three shadows gives the true area, $|\vec{b}\times\vec{c}|^2 = \sum(\text{minors})^2$ — Pythagoras in [[Euclidean space]], for areas instead of lengths.
- Swapping two rows flipping the sign is anticommutativity from [[Vector Product]] seen inside a single $2\times2$.
- A determinant of zero means the rows are dependent — the same degeneracy that $\det(A - \lambda I) = 0$ exploits to find eigenvalues, the still-open characteristic polynomial of [[eigen]].
- This is the arithmetic a renderer does for the face normal $(B-A)\times(C-A)$ on the index triples of [[Cylinder]] and [[Sphere]].
- Adding a third vector turns the area into a volume: the same grid with a real third row instead of $\hat{\imath}, \hat{\jmath}, \hat{k}$ is the scalar triple product of [[Volumes and the scalar triple product]].
- Part of [[Home]].
