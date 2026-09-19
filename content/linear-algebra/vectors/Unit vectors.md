The unit vector takes the following form:
$$
\hat{\mathbf{a}} = \frac{\mathbf{a}}{|\mathbf{a}|} = \left(\frac{a_x}{a}, \frac{a_y}{a}, \frac{a_z}{a}\right) = (\cos\theta_x, \cos\theta_y, \cos\theta_z)
$$
We can prove that:
$$
\hat{\mathbf{a}} = \left(\frac{a_x}{a}, \frac{a_y}{a}, \frac{a_z}{a}\right)
$$
Has a magnitude of one with the following:
$$
\hat{a} = \sqrt{\left(\frac{a_x}{a}\right)^{2} + \left(\frac{a_y}{a}\right)^{2} + \left(\frac{a_z}{a}\right)^{2}} = \frac{\sqrt{a_x^{2} + a_y^{2} + a_z^{2}}}{a} = \frac{|\mathbf{a}|}{a} = 1
$$
As the unit vector has a magnitude of one, we can represent it via the following angle:
$$
\cos^2\theta_x + \cos^2\theta_y + \cos^2\theta_z = 1
$$
so only two are independent. Given $\theta_x$ and $\theta_y$, recover $\theta_z$ as follows.

## Steps

**1. Subtract the two stored terms.**
$$
\cos^2\theta_z = 1 - \cos^2\theta_x - \cos^2\theta_y
$$
**2. Take the square root.** The right side is a square, so both roots are admissible:
$$
\cos\theta_z = \pm\sqrt{1 - \cos^2\theta_x - \cos^2\theta_y}
$$
**3. Invert the cosine.** Since $\theta_z \in [0,\pi]$, $\arccos$ is a genuine inverse there:
$$
\theta_z = \arccos\!\left(\pm\sqrt{1 - \cos^2\theta_x - \cos^2\theta_y}\right)

$$
The two branches are reflections about the $xy$-plane: the $+$ root gives
$\theta_z \in [0, \pi/2]$, and the $-$ root gives exactly $\pi - \theta_z$.

## Record layout

One located vector (origin + direction + magnitude):

| field | native (f64) | packed |
| --- | --- | --- |
| origin x, y, z | 24 B | 12 B (3 × f32) |
| direction | 24 B | 4 B (2 × u16 angle) |
| magnitude | (folded into direction) | 4 B (f32) |
| **total** | **48 B** | **20 B** |

Intermediate option — everything f32, no quantization: **24 B** (origin 3 × f32,
direction 3 × f32 with magnitude as the vector's length). That is 2× of the 2.4×,
with no trig, no hemisphere bit and no pole case.

## Angular resolution vs bits

Angular resolution for an $n$-bit-per-angle scheme is approximately
$\sqrt{4\pi}/2^{n}$ rad:

| bits/angle | direction size | angular error | linear error at 1 m |
| --- | --- | --- | --- |
| 8 | 2 B | $1.4\times10^{-2}$ rad (0.79°) | 14 mm |
| 12 | 3 B | $8.7\times10^{-4}$ rad (0.050°) | 870 µm |
| 16 | 4 B | $5.4\times10^{-5}$ rad (11 arcsec) | 54 µm |
| 21 | ~5.25 B | $1.7\times10^{-6}$ rad | 1.7 µm |
| f64 | 24 B | ~$10^{-16}$ rad | ~$10^{-13}$ m |

16 bits per angle is ~11 orders of magnitude coarser than f64.

## Connections
- Part of [[Linear Algebra]]. This picks up exactly where [[Basic Operations]] stops — $\hat{\mathbf{a}} = \mathbf{a}/|\mathbf{a}|$ and the magnitude $\sqrt{a_x^2 + a_y^2 + a_z^2}$ are both defined there; here the components get read as angles.
- The proof that $|\hat{\mathbf{a}}| = 1$ is the Pythagorean metric of [[Euclidean space]] divided through by $a$ — it only holds because distance in that space is the straight-line one.
- Splitting a stored vector into *origin* plus *direction* plus *magnitude* is the [[Displacement vs Position Vectors]] distinction made concrete: the origin is a position vector, the direction-and-magnitude pair is a displacement.
- A normalised direction is half of what defines a line — a point and a direction — so this is the vector form behind [[Lines]].
- Each direction cosine is a dot product against a basis vector, $\cos\theta_x = \hat{\imath}\cdot\hat{\mathbf{a}}$, and the unit vector is what makes projection $\hat{a}\cdot\vec{b}$ meaningful — [[Scalar Product]].
- One way to *produce* a unit vector rather than normalise one: the $\hat{n}$ of [[Vector Product]] is the unit normal to the plane of two vectors.
- A direction that arrives already normalised rather than needing it: the ring of [[Cylinder]] is $R(\cos\theta, \sin\theta, 0)$, a unit vector scaled to the radius.
- On the unit sphere of [[Sphere]] every vertex position *is* a unit vector, and therefore its own outward normal — no separate normal buffer is needed.
- Recovering $\theta_z = \arccos(\pm\sqrt{\dots})$ leans on $\arccos$ being a genuine inverse on $[0,\pi]$, the principal-branch machinery from [[Trigonometry]]. The identity $\cos^2\theta_x + \cos^2\theta_y + \cos^2\theta_z = 1$ is the 3D direction-cosine analogue of $\cos^2\theta + \sin^2\theta = 1$.
- The record layout and quantisation tables are here because [[CAD Geometry]] is what stores millions of these — the endpoint tangents $\mathbf{B}'(0) = n(\mathbf{P}_1 - \mathbf{P}_0)$ of [[Bezier Curves]] are exactly the directions being packed.
