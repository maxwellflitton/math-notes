---
tags: [maths, vectors, linear-algebra]
type: topic
---
## Products of vectors

There are two ways of forming a product of two vectors:

- **Scalar product** (a.k.a. **dot product**) — returns a *number*
- **Vector product** (a.k.a. **cross product**) — returns a *vector*

> [!warning] Naming
> "Scalar product" and "dot product" are two names for the **same** operation. The genuine second option is the vector/cross product. → [[Vector Product]]

---

## Definition

$$
\vec{a}\cdot\vec{b} = |\vec{a}|\,|\vec{b}|\cos\theta
\qquad (0 \le \theta \le \pi)
$$

$\theta$ is the angle between the two vectors, measured in the plane they span. Restricting to $[0, \pi]$ makes it unambiguous — there is no signed direction to worry about, unlike the cross product.

### Sign carries meaning

| $\vec{a}\cdot\vec{b}$ | $\theta$ | interpretation |
|---|---|---|
| $> 0$ | acute | roughly the same direction |
| $= 0$ | $\pi/2$ | **orthogonal** |
| $< 0$ | obtuse | roughly opposing directions |

---

## Algebraic properties

**Commutative**

$$
\vec{a}\cdot\vec{b} = \vec{b}\cdot\vec{a}
$$

**Distributive over addition**

$$
\vec{a}\cdot(\vec{b}+\vec{c}) = \vec{a}\cdot\vec{b} + \vec{a}\cdot\vec{c}
$$

**Linear with respect to scalar multiplication**

$$
(\lambda\vec{a})\cdot\vec{b} = \vec{a}\cdot(\lambda\vec{b}) = \lambda(\vec{a}\cdot\vec{b})
$$

Together these make the dot product **bilinear** — linear in each argument separately. That is the property everything else rests on.

### Worked example 1 — difference of squares

$$
(\vec{a}+\vec{b})\cdot(\vec{a}-\vec{b})
= \vec{a}\cdot\vec{a} - \vec{a}\cdot\vec{b} + \vec{b}\cdot\vec{a} - \vec{b}\cdot\vec{b}
$$

Since $\vec{a}\cdot\vec{b} = \vec{b}\cdot\vec{a}$, the middle terms cancel:

$$
\vec{b}\cdot\vec{a} - \vec{a}\cdot\vec{b} = 0
$$

$$
\boxed{\;(\vec{a}+\vec{b})\cdot(\vec{a}-\vec{b}) = \vec{a}\cdot\vec{a} - \vec{b}\cdot\vec{b} = |\vec{a}|^{2} - |\vec{b}|^{2}\;}
$$

### Worked example 2 — expanding a squared magnitude

$$
|\vec{a}+\vec{b}|^{2} = (\vec{a}+\vec{b})\cdot(\vec{a}+\vec{b})
= \vec{a}\cdot\vec{a} + \vec{a}\cdot\vec{b} + \vec{b}\cdot\vec{a} + \vec{b}\cdot\vec{b}
$$

$$
\boxed{\;|\vec{a}+\vec{b}|^{2} = |\vec{a}|^{2} + 2(\vec{a}\cdot\vec{b}) + |\vec{b}|^{2}\;}
$$

The step doing the work here is $\vec{a}\cdot\vec{a} = |\vec{a}|^{2}$ — **length is defined by the dot product**, not the other way round.

---

## Component form

For the standard basis, orthonormality gives:

$$
\hat{\imath}\cdot\hat{\imath} = \hat{\jmath}\cdot\hat{\jmath} = \hat{k}\cdot\hat{k} = 1
\qquad
\hat{\imath}\cdot\hat{\jmath} = \hat{\jmath}\cdot\hat{k} = \hat{k}\cdot\hat{\imath} = 0
$$

Expanding by bilinearity:

$$
\vec{a}\cdot\vec{b} =
a_x\hat{\imath}\cdot(b_x\hat{\imath} + b_y\hat{\jmath} + b_z\hat{k})
+ a_y\hat{\jmath}\cdot(b_x\hat{\imath} + b_y\hat{\jmath} + b_z\hat{k})
+ a_z\hat{k}\cdot(b_x\hat{\imath} + b_y\hat{\jmath} + b_z\hat{k})
$$

Every cross-term has $\theta = 90^{\circ}$, so $\cos\theta = 0$ and it vanishes. Only the matching components survive:

$$
\boxed{\;\vec{a}\cdot\vec{b} = a_x b_x + a_y b_y + a_z b_z\;}
$$

Equating the two definitions gives the angle formula:

$$
\cos\theta = \frac{\vec{a}\cdot\vec{b}}{|\vec{a}|\,|\vec{b}|}
= \frac{a_x b_x + a_y b_y + a_z b_z}{|\vec{a}|\,|\vec{b}|}
$$

### Worked example — angle between two vectors

$$
\vec{a} = 2\hat{\imath} - 3\hat{\jmath} + \hat{k}
\qquad
\vec{b} = -\hat{\imath} + 2\hat{\jmath} + 4\hat{k}
$$

Magnitudes:

$$
|\vec{a}| = \sqrt{2^{2} + (-3)^{2} + 1^{2}} = \sqrt{14}
\qquad
|\vec{b}| = \sqrt{(-1)^{2} + 2^{2} + 4^{2}} = \sqrt{21}
$$

Dot product:

$$
\vec{a}\cdot\vec{b} = (2)(-1) + (-3)(2) + (1)(4) = -2 - 6 + 4 = -4
$$

Angle:

$$
\cos\theta = \frac{-4}{\sqrt{14}\sqrt{21}} = \frac{-4}{\sqrt{294}} = \frac{-4}{7\sqrt{6}} \approx -0.2333
$$

$$
\theta = \arccos\!\left(\frac{-4}{7\sqrt{6}}\right) \approx 1.806\ \text{rad} \approx 103.5^{\circ}
$$

> [!tip] Keep the sign
> $\cos\theta$ is the **negative** number; $1.806$ is $\theta$ itself. The negative sign is what tells you $\theta$ is obtuse — drop it and you lose the only geometric information in the result.

Since $\vec{a}\cdot\vec{b} \ne 0$, the two vectors are **not** orthogonal.

---

## Components with respect to an orthonormal basis

Given perpendicular unit vectors $\hat{u}$ and $\hat{v}$, any $\vec{a}$ in the plane can be written

$$
\vec{a} = \alpha\hat{u} + \beta\hat{v}
$$

To extract $\alpha$, dot both sides with $\hat{u}$:

$$
\hat{u}\cdot\vec{a} = \hat{u}\cdot(\alpha\hat{u} + \beta\hat{v})
= \alpha\,(\hat{u}\cdot\hat{u}) + \beta\,(\hat{u}\cdot\hat{v})
$$

Because $\hat{u}\cdot\hat{u} = 1$ and $\hat{u}\cdot\hat{v} = 0$:

$$
a_u = \hat{u}\cdot\vec{a} = \alpha
\qquad\text{and likewise}\qquad
a_v = \hat{v}\cdot\vec{a} = \beta
$$

$$
\boxed{\;\vec{a} = (\hat{u}\cdot\vec{a})\,\hat{u} + (\hat{v}\cdot\vec{a})\,\hat{v}\;}
$$

**This is the payoff of an orthonormal basis.** The coordinates fall out of one dot product each — no simultaneous equations to solve.

### Worked example

$$
\hat{u} = \left(\tfrac{1}{\sqrt{2}}, \tfrac{1}{\sqrt{2}}\right)
\qquad
\hat{v} = \left(-\tfrac{1}{\sqrt{2}}, \tfrac{1}{\sqrt{2}}\right)
\qquad
\vec{a} = (1, 3)
$$

$$
a_u = \hat{u}\cdot\vec{a} = \frac{1}{\sqrt{2}} + \frac{3}{\sqrt{2}}
= \frac{1}{\sqrt{2}}(1+3) = \frac{4}{\sqrt{2}}
= \frac{4}{\sqrt{2}}\cdot\frac{\sqrt{2}}{\sqrt{2}} = \frac{4\sqrt{2}}{2} = 2\sqrt{2}
$$

$$
a_v = \hat{v}\cdot\vec{a} = \frac{-1}{\sqrt{2}} + \frac{3}{\sqrt{2}}
= \frac{2}{\sqrt{2}} = \frac{2\sqrt{2}}{2} = \sqrt{2}
$$

$$
\boxed{\;\vec{a} = 2\sqrt{2}\,\hat{u} + \sqrt{2}\,\hat{v}\;}
$$

**Check:** $2\sqrt{2}\,\hat{u} = (2,2)$ and $\sqrt{2}\,\hat{v} = (-1,1)$, summing to $(1,3)$. ✓

> [!note] Basis vs. datum
> A **basis** fixes the *directions* of the axes; an **origin** fixes where zero is. You need both to pin down a point. The vector itself is unchanged by a change of basis — only its *numbers* change. Choosing a good basis is what makes a problem easy or horrible.

---

## Projection along a direction

The **scalar component** of $\vec{b}$ along $\vec{a}$:

$$
\text{comp}_{\vec{a}}\vec{b} = \frac{\vec{a}\cdot\vec{b}}{|\vec{a}|} = \hat{a}\cdot\vec{b}
$$

This splits a vector into "the part along this direction" and "the part perpendicular to it" — the same decomposition move as [[eigen]].

### Worked example — displacement

$$
\vec{a} = 2\hat{\imath} - 3\hat{\jmath} + \hat{k}
\qquad
\vec{b} = -\hat{\imath} + 2\hat{\jmath} + 4\hat{k}
$$

$$
\vec{a} + 2\vec{b} = (2-2)\hat{\imath} + (-3+4)\hat{\jmath} + (1+8)\hat{k} = \hat{\jmath} + 9\hat{k}
$$

The displacement from the origin to the point $(1,1,1)$ is $\vec{r} = \hat{\imath} + \hat{\jmath} + \hat{k}$, with unit vector

$$
\hat{r} = \frac{1}{\sqrt{3}}\left(\hat{\imath} + \hat{\jmath} + \hat{k}\right)
$$

The component of $\vec{a} + 2\vec{b}$ in the direction $\hat{r}$:

$$
\hat{r}\cdot(\vec{a}+2\vec{b})
= \frac{1}{\sqrt{3}}\left(\hat{\imath}+\hat{\jmath}+\hat{k}\right)\cdot\left(\hat{\jmath}+9\hat{k}\right)
= \frac{1}{\sqrt{3}}(0 + 1 + 9) = \frac{10}{\sqrt{3}} = \frac{10\sqrt{3}}{3} \approx 5.774
$$

---

## Worked example — expressing a vector in an orthonormal basis

Given the three unit vectors

$$
\hat{u} = \tfrac{1}{\sqrt{2}}(1, 0, 1)
\qquad
\hat{v} = \tfrac{1}{\sqrt{2}}(1, 0, -1)
\qquad
\hat{w} = (0, 1, 0)
$$

express $\vec{a} = (2, 1, 0)$ as a linear combination of them.

> [!note] Check the basis first
> The method below only works because these are **orthonormal**: each has unit length, and $\hat{u}\cdot\hat{v} = \hat{u}\cdot\hat{w} = \hat{v}\cdot\hat{w} = 0$.

We want

$$
\vec{a} = a_u\hat{u} + a_v\hat{v} + a_w\hat{w}
$$

and each coefficient is a single dot product:

$$
a_u = \vec{a}\cdot\hat{u} = \frac{1}{\sqrt{2}}(2,1,0)\cdot(1,0,1)
= \frac{2}{\sqrt{2}} = \frac{2}{\sqrt{2}}\cdot\frac{\sqrt{2}}{\sqrt{2}} = \frac{2\sqrt{2}}{2} = \sqrt{2}
$$

$$
a_v = \vec{a}\cdot\hat{v} = \frac{1}{\sqrt{2}}(2,1,0)\cdot(1,0,-1)
= \frac{2}{\sqrt{2}} = \sqrt{2}
$$

$$
a_w = \vec{a}\cdot\hat{w} = (2,1,0)\cdot(0,1,0) = 1
$$

$$
\boxed{\;\vec{a} = \sqrt{2}\,\hat{u} + \sqrt{2}\,\hat{v} + \hat{w}\;}
$$

**Check by reconstruction:**

$$
\sqrt{2}\cdot\tfrac{1}{\sqrt{2}}(1,0,1) + \sqrt{2}\cdot\tfrac{1}{\sqrt{2}}(1,0,-1) + (0,1,0)
= (1,0,1) + (1,0,-1) + (0,1,0) = (2,1,0) \;\checkmark
$$

Note the $\hat{u}$ and $\hat{v}$ contributions cancel in the $z$ component and reinforce in $x$ — which is why $\vec{a}$ has no $z$ part despite both basis vectors having one.

## Why the scalar product matters

Start with nothing but lists of numbers — no length, no angle. Define this one bilinear operation and the **entire geometry of the space falls out of it**:

$$
|\vec{a}| = \sqrt{\vec{a}\cdot\vec{a}}
\qquad
\cos\theta = \frac{\vec{a}\cdot\vec{b}}{|\vec{a}||\vec{b}|}
\qquad
\vec{a}\perp\vec{b} \iff \vec{a}\cdot\vec{b} = 0
$$

That is why it earns a name. It is the bridge from algebra to geometry. The scalar product essentially a machine that eats two vectors and spits out a single number in a linear way. You collapse the vector into a number and this number answers the question, how much do these two vectors agree on each other. But you can reconstruct the vectors again but be careful, the numbers are meaningless without the frame they are measured in if you want to reconstruct it. 

### It generalises beyond arrows

A vector is anything you can add and scale sensibly — which includes **functions**. On a function space the dot product becomes an integral:

$$
\langle f, g\rangle = \int_{a}^{b} f(x)\,g(x)\,dx
$$

Orthogonality still means the product is zero. This is the entire foundation of [[Fourier Series]]. → [[Function Spaces]]


---

## Connections
- Part of [[Linear Algebra]], and the operation [[Basic Operations]] stops short of: that note defines $|\vec{a}| = \sqrt{a_x^2+a_y^2+a_z^2}$ by assertion, while here it *falls out* of $|\vec{a}| = \sqrt{\vec{a}\cdot\vec{a}}$.
- That identity is why [[Euclidean space]] has the metric it does — the straight-line distance is the dot product's, and a different inner product would give a different geometry.
- The direction cosines of [[Unit vectors]] are dot products in disguise: $\cos\theta_x = \hat{\imath}\cdot\hat{a}$, and $\text{comp}_{\vec{a}}\vec{b} = \hat{a}\cdot\vec{b}$ needs that normalisation to work at all.
- Two unit vectors at angles $A$ and $B$ to the $x$-axis have $\hat{a}\cdot\hat{b} = \cos A\cos B + \sin A\sin B$, which by the definition is $\cos(A-B)$ — a two-line proof of the [[compound angle formula|compound angle formulae]].
- Orthogonality reads geometry off algebra: $ax + by = c$ in [[Lines]] is $\vec{n}\cdot\vec{r} = c$, the set of points whose projection onto the normal $\vec{n}$ is fixed.
- Projection splits a vector into a part along a direction and a part perpendicular to it — the decomposition move of [[eigen]], where an orthonormal basis is the case that makes it free.
- The displacement worked example is the [[Displacement vs Position Vectors]] distinction: $\hat{r}$ is a direction taken from a position vector, and the thing projected is a displacement.
- Testing tangent continuity where two curves join is a dot-product test on their end tangents — [[Bezier Curves]], [[CAD Geometry]].
- The function-space inner product $\langle f, g\rangle = \int_a^b f g\,dx$ is an ordinary definite integral: [[Calculus]].
- The other product: [[Vector Product]] returns a vector instead of a number, is maximal where this one vanishes, and measures spanned area rather than alignment.
- Dot gives $\cos\theta$, the $2\times2$ determinant gives $\sin\theta$ with the same $|\vec{u}||\vec{v}|$ in front, and `atan2(det, dot)` recovers the signed angle — the [[Determinant (cofactor) form of the cross product|determinant form of the cross product]].
- Over 0/1 vectors the dot product is AND-then-count, so two bitsets are orthogonal exactly when they share no bits — the deny-list check in [[Bitset Auth]].
- Part of [[Home]].

## Open
- [ ] [[Fourier Series]], [[Function Spaces]] — both notes exist as empty stubs; the orthogonality-of-functions material is still to be written.
