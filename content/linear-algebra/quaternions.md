---
tags: [maths, quaternions, rotation, graphics, cad]
related: "[[Linear Algebra]]"
created: 2026-10-18
---

# Quaternions

Part of [[Linear Algebra]]. The rotation representation that replaces a $3 \times 3$ matrix with four numbers, built by bundling the dot and cross products of [[Scalar Product]] and [[Vector Product]] into a single multiplication.

## Coordinates

A quaternion is a scalar and a 3D vector:

$$
q = (s, \vec v) = s + v_x\,\hat\imath + v_y\,\hat\jmath + v_z\,\hat k
$$

## The Hamilton product

The dot product and cross product get bundled into one operation. Multiplying two quaternions out:

$$
(a, \vec u)(b, \vec w) = (a + u_x\hat\imath + u_y\hat\jmath + u_z\hat k)(b + w_x\hat\imath + w_y\hat\jmath + w_z\hat k)
$$

As the vector basis elements are imaginary, we have the rules:

$$
\hat\imath^2 = \hat\jmath^2 = \hat k^2 = -1,\qquad
\hat\imath\hat\jmath = \hat k,\quad \hat\jmath\hat k = \hat\imath,\quad \hat k\hat\imath = \hat\jmath,\quad \hat\jmath\hat\imath = -\hat k
$$

(and likewise $\hat k\hat\jmath = -\hat\imath$, $\hat\imath\hat k = -\hat\jmath$ — swapping the order flips the sign).

Expanding is lengthy, but under those rules the components come out as:

| Part         | Expression                            |
| ------------ | ------------------------------------- |
| scalar       | $ab - u_x w_x - u_y w_y - u_z w_z$    |
| $\hat\imath$ | $a w_x + b u_x + (u_y w_z - u_z w_y)$ |
| $\hat\jmath$ | $a w_y + b u_y + (u_z w_x - u_x w_z)$ |
| $\hat k$     | $a w_z + b u_z + (u_x w_y - u_y w_x)$ |

The scalar part is $ab - \vec u \cdot \vec w$. The first two terms of each vector component are $a\vec w + b\vec u$, and the brackets are exactly the cross product:

$$
\vec u \times \vec w = \big(u_y w_z - u_z w_y,\; u_z w_x - u_x w_z,\; u_x w_y - u_y w_x\big)
$$

So the whole product collapses to:

$$
(a, \vec u)(b, \vec w) = \big(ab - \vec u\cdot\vec w,\;\; a\vec w + b\vec u + \vec u \times \vec w\big)
$$

Because of the cross term, the product does **not** commute: $qp \neq pq$.

## Norm

$$
|q|^2 = s^2 + v_x^2 + v_y^2 + v_z^2
$$

Just Pythagoras in four dimensions.

## Conjugate

Keep the scalar, negate the vector part:

$$
\bar q = (s, -\vec v)
$$

A direct analogue of the complex conjugate, where $a + bi$ becomes $a - bi$. Multiplying a quaternion by its own conjugate:

$$
q\bar q = (s, \vec v)(s, -\vec v) = \big(s^2 + \vec v\cdot\vec v,\;\; -s\vec v + s\vec v - \vec v \times \vec v\big)
$$

The middle terms cancel and $\vec v \times \vec v = 0$, so:

$$
q\bar q = (|q|^2, \vec 0) \quad\Rightarrow\quad q\bar q = |q|^2
$$

Compare $z\bar z = |z|^2$ for complex numbers — the same structure at work.

## Inverse

Since $q\bar q = |q|^2$ is a real number, divide both sides by it:

$$
q\,\frac{\bar q}{|q|^2} = 1 \quad\Rightarrow\quad q^{-1} = \frac{\bar q}{|q|^2}
$$

And if $|q| = 1$ (a **unit** quaternion) then $q^{-1} = \bar q$. The inverse is free — no division — which is why rotations are always stored as unit quaternions.

## Rotation

For a unit quaternion, a rotation by angle $\theta$ about the unit axis $\hat n$ is:

$$
q(\hat n, \theta) = \Big(\cos\tfrac{\theta}{2},\; \hat n \sin\tfrac{\theta}{2}\Big)
$$

Its conjugate (and therefore its inverse) is:

$$
\bar q = \Big(\cos\tfrac{\theta}{2},\; -\hat n \sin\tfrac{\theta}{2}\Big)
$$

Rotating a vector means embedding it as a **pure quaternion** (zero scalar part), $\vec v \mapsto (0, \vec v)$, and sandwiching it:

$$
\vec v\,' = q\,\vec v\,\bar q
$$

The result is again a pure quaternion whose vector part is $\vec v$ rotated by $\theta$ about $\hat n$.

> [!note] Why the half angle
> The sandwich applies $q$ on the left and $\bar q$ on the right, and each contributes half the rotation. A single $q$ would be a rotation by $\theta/2$ in 4D; the conjugation folds it into a full $\theta$ in the 3D subspace of pure quaternions.

> [!tip] Why this matters for the CAD engine
>
> - Four floats instead of nine for a rotation; composing rotations is one Hamilton product instead of a matrix multiply.
> - Unit quaternions stay numerically close to a valid rotation under floating-point drift and can be cheaply renormalised; rotation matrices drift into shears.
> - No gimbal lock, and slerp between two unit quaternions gives smooth interpolation for animation and constraint solving.

---

## Connections

- Part of [[Linear Algebra]]. The Hamilton product _is_ the two vector products of this area in one operation: the scalar part is the dot product of [[Scalar Product]] and the bracketed terms are the cross product of [[Vector Product]]. Non-commutativity is inherited directly — $\vec u \times \vec w = -\vec w \times \vec u$ is the only asymmetric term, which is why $qp \neq pq$.
- The component formula $\vec u \times \vec w = (u_yw_z - u_zw_y,\ \ldots)$ used above is the cofactor expansion of [[Determinant (cofactor) form of the cross product]], and $\vec v \times \vec v = \vec 0$ — the step that collapses $q\bar q$ — is that note's degenerate determinant with two equal rows.
- $|q|^2 = s^2 + v_x^2 + v_y^2 + v_z^2$ is Pythagoras one dimension further out than [[Euclidean space]] states it, and normalising to $|q| = 1$ is the operation of [[Unit vectors]] applied to a 4-vector. The rotation axis $\hat n$ is itself a unit vector in the ordinary 3D sense.
- The whole conjugate-and-norm structure is the complex one: $\bar q = (s, -\vec v)$ against $\overline{a + bi} = a - bi$, and $q\bar q = |q|^2$ against $z\bar z = |z|^2$. Complex numbers rotate the plane and quaternions rotate space — the planar case is in [[Complex conjugate roots]].
- The alternative to all of this is a rotation matrix: $R(\alpha)$ in [[Matrix Overview]], composed by matrix product and inverted as $R^{-1}(\alpha) = R(-\alpha)$. The quaternion mirrors each piece — compose by Hamilton product, invert by conjugate — in four floats instead of nine, and $q^{-1} = \bar q$ for a unit quaternion is the exact analogue of a rotation matrix's inverse being its transpose. The general $\frac{1}{\det A}C^T$ machinery of [[Determinants and inverses for matrices]] is what both are avoiding.
- Composing two rotations about the same axis multiplies out to $q(\hat n, \alpha)\,q(\hat n, \beta) = q(\hat n, \alpha + \beta)$, and the proof is $\cos\tfrac{\alpha}{2}\cos\tfrac{\beta}{2} - \sin\tfrac{\alpha}{2}\sin\tfrac{\beta}{2} = \cos\tfrac{\alpha+\beta}{2}$ — the [[compound angle formula|compound angle formulae]] again, exactly as they prove $R(\alpha)R(\beta) = R(\alpha+\beta)$ for matrices. The half angles cancel, which is the cleanest sign that $\theta/2$ is the natural parameter.
- $\hat n$ is the one direction the rotation leaves alone, i.e. the eigenvector of the equivalent matrix with eigenvalue $1$ — [[eigen]]. The pure quaternions $(0, \vec v)$ are the 3D subspace the sandwich $q\vec v\bar q$ maps to itself, so the axis-plus-angle reading and the eigen reading are the same decomposition.
- Where it lands in the engine: the camera orientation built by hand in [[Ortho derivation]] and multiplied into $PV$ in [[Ortho Application]] is a rotation that can be stored and interpolated as a quaternion and converted to a matrix only at upload time, which is the drift argument in the tip above. The geometry being oriented is [[CAD Geometry]], and the meshes are those of [[Cylinder]] and [[Sphere]].
- Slerp interpolates along a great arc of the unit sphere in 4D rather than along a straight line, the rotational counterpart to interpolating control points in [[Bezier Curves]]; both are a parameter $t \in [0,1]$ walked between endpoints, one on a sphere and one in flat space.
- Part of [[Home]].

## Open

- [ ] Proof of the sandwich: show $q\vec v\bar q$ is pure, preserves length, and rotates by $\theta$ about $\hat n$ — the callout asserts it rather than deriving it.
- [ ] Slerp written out, with the great-arc geometry and why lerp-then-normalise is not the same thing.
- [ ] Quaternion to $3 \times 3$ matrix conversion, and back, with the trace trick for the reverse direction.
- [ ] The double cover: $q$ and $-q$ give the same rotation, and what that costs when interpolating or comparing orientations.
- [ ] Euler angles and gimbal lock written out concretely, so the "no gimbal lock" claim has a counterexample behind it.
