The **vector product** (or **cross product**) is the second way of forming a product of two vectors. Unlike the [[Scalar Product]], it returns a *vector*.

---

## Definition

$$
\vec{a}\times\vec{b} = \left(|\vec{a}|\,|\vec{b}|\sin\theta\right)\hat{n}
$$

Two parts to read separately:

- **Magnitude** $|\vec{a}||\vec{b}|\sin\theta$ — the area of the **parallelogram** spanned by the two vectors.
- **Direction** $\hat{n}$ — the unit vector perpendicular to the plane containing $\vec{a}$ and $\vec{b}$, with the sense given by the **right-hand rule**.

> [!important] $\hat{n}$ is perpendicular to the *plane*, not "at $\tfrac{\pi}{2}$ from the plane"
> $\hat{n}$ sits at $\tfrac{\pi}{2}$ to **both vectors** simultaneously, which is the same as saying it is normal to the plane they span. The right-hand rule is what picks between the two opposite directions available.

---

## Behaviour of the magnitude

| $\theta$ | $\sin\theta$ | area | result |
|---|---|---|---|
| $0$ (parallel) | $0$ | none | $\vec{0}$ |
| $\tfrac{\pi}{2}$ (perpendicular) | $1$ | maximal | $|\vec{a}||\vec{b}|\,\hat{n}$ |
| $\pi$ (antiparallel) | $0$ | none | $\vec{0}$ |

At $\theta = \tfrac{\pi}{2}$ the parallelogram becomes a **rectangle** of area $|\vec{a}||\vec{b}|$ — the largest area obtainable from two fixed lengths. (It is only a *square* in the special case $|\vec{a}| = |\vec{b}|$.)

At $\theta = 0$ the vectors are parallel, the parallelogram is flat, and the area vanishes.

> [!note] Any orientation is fine
> Whatever directions $\vec{a}$ and $\vec{b}$ point in, they span *some* plane, and $\hat{n}$ is normal to that plane. There is no requirement for them to lie in a particular coordinate plane. The **only** degenerate case is parallel vectors — then no unique plane exists and the product collapses to $\vec{0}$.

This gives a useful test, exactly dual to the orthogonality test for the dot product:

$$
\vec{a}\times\vec{b} = \vec{0} \iff \vec{a} \parallel \vec{b}
\qquad\text{(for non-zero vectors)}
\qquad\text{and in particular}\qquad
\vec{a}\times\vec{a} = \vec{0}
$$

---

## Algebraic properties

**Distributive over addition**

$$
\vec{a}\times(\vec{b}+\vec{c}) = (\vec{a}\times\vec{b}) + (\vec{a}\times\vec{c})
$$

**Linear with respect to scalar multiplication**

$$
\vec{a}\times(\lambda\vec{b}) = \lambda(\vec{a}\times\vec{b})
$$

**NOT commutative — it is anticommutative**

$$
\vec{a}\times\vec{b} \ne \vec{b}\times\vec{a}
\qquad\text{instead}\qquad
\boxed{\;\vec{a}\times\vec{b} = -\,\vec{b}\times\vec{a}\;}
$$

Swapping the operands **flips the normal**. This is not a technicality — it is why winding order determines which side of a face is the "front".

> [!warning] Also not associative
> $(\vec{a}\times\vec{b})\times\vec{c} \ne \vec{a}\times(\vec{b}\times\vec{c})$ in general. Brackets always matter.

---

## Basis vectors

$$
\hat{\imath}\times\hat{\jmath} = \left(|1||1|\sin\tfrac{\pi}{2}\right)\hat{k} = \hat{k}
$$

The full cycle, each following the right-hand rule:

$$
\hat{\imath}\times\hat{\jmath} = \hat{k}
\qquad
\hat{\jmath}\times\hat{k} = \hat{\imath}
\qquad
\hat{k}\times\hat{\imath} = \hat{\jmath}
$$

Reverse any of these and the sign flips. Cross any basis vector with itself and you get $\vec{0}$.

---

## Contrast with the scalar product

|  | scalar product | vector product |
|---|---|---|
| returns | number | vector |
| formula | $|\vec{a}||\vec{b}|\cos\theta$ | $(|\vec{a}||\vec{b}|\sin\theta)\hat{n}$ |
| maximal when | parallel | perpendicular |
| zero when | perpendicular | parallel |
| symmetry | commutative | anticommutative |
| measures | alignment | spanned area |

They are complements: one detects agreement, the other detects independence.


---

## Connections
- Part of [[Linear Algebra]], and the other half of the pair opened in [[Scalar Product]] — that note's orthogonality test ($\vec{a}\cdot\vec{b} = 0$) and this note's parallel test ($\vec{a}\times\vec{b} = \vec{0}$) are exact duals.
- The two are tied together by Pythagoras: $|\vec{a}\times\vec{b}|^2 + (\vec{a}\cdot\vec{b})^2 = |\vec{a}|^2|\vec{b}|^2$, which is just $\sin^2\theta + \cos^2\theta = 1$ scaled up — see [[Trigonometry]].
- $\hat{n}$ is a unit vector in the sense of [[Unit vectors]]: the cross product supplies a *direction* (the normal) and a *magnitude* (the area) separately, which is exactly the split that note is about.
- The vectors being multiplied are the component-form ones of [[Basic Operations]], and the plane they span lives in [[Euclidean space]] — the cross product only exists in three dimensions, where "normal to a plane" picks out a single line.
- $\vec{a} \parallel \vec{b} \iff \vec{a}\times\vec{b} = \vec{0}$ is the cleanest parallel test for the direction vectors of [[Lines]], and the reason $\vec{a}\times\vec{a} = \vec{0}$.
- The same test states the eigenvector condition geometrically: $\mathbf{v}$ is an eigenvector of $A$ exactly when $A\mathbf{v} \times \mathbf{v} = \vec{0}$, i.e. $A$ leaves its direction alone — [[eigen]].
- Anticommutativity is why winding order decides which face of a surface is the front, and the normal is what lighting and offsetting are computed against — [[CAD Geometry]], with the tangent directions coming from [[Bezier Curves]].
- The faces of [[Cylinder]] are index triples written in a fixed order; that order is what a renderer crosses two edge vectors to turn into an outward normal.
- Part of [[Home]].

## Open
- [ ] The component/determinant form $\vec{a}\times\vec{b} = (a_yb_z - a_zb_y,\ a_zb_x - a_xb_z,\ a_xb_y - a_yb_x)$ — the counterpart to the component form derived in [[Scalar Product]].

