---
tags:
  - cad
  - maths
  - curves
  - continuity
  - curvature
date: 2026-10-24
---

# Curve continuity in CAD

A curve map is defined as the following:

$$c : [a, b] \to \mathbb{R}^3$$

In CAD we use piecewise polynomial or rational maps because they are cheap, stable, and controllable through points — what those two families are, and why the rational one is needed for circles, is in [[Piecewise polynomials and rational maps]]. There are two ways of achieving continuity: **parametric** and **geometric**.

## Parametric continuity

Our first one is parametric. This means that:

$$c_1^{(r)}(1) = c_2^{(r)}(0), \qquad r = 0, \dots, k \quad (k = \text{highest derivative})$$

This means that the end of the $c_1$ curve must equal the start of $c_2$. Note that $r$ is the derivative, so when $r = 0$ we have the following:

$$c_1^{(0)}(1) = c_2^{(0)}(0)$$

This has no differentiation, therefore the curves just have to meet, like the following:

![[g0-join.png|G0 join: a blue curve c1 and a red curve c2 meeting at a marked point, with a visible corner where they join]]

_Figure 1. $G^0 / C^0$ — the curves share the join point and nothing else, so the tangent jumps and the join reads as a corner._

For our first derivative $k = 1$, giving us the following:

$$c_1'(1) = c_2'(0) \qquad (r = 1)$$

Remember that we can present its tangent vector as the following:

$$c'(t) = \begin{bmatrix} x'(t) \\ y'(t) \\ z'(t) \end{bmatrix}$$

This condition can be demonstrated with the following curve:

![[g1-join.png|G1 join: a blue curve c1 and a red curve c2 meeting at a marked point, with one green arrow lying along both curves as a shared tangent]]

_Figure 2. $G^1 / C^1$ — the same join with the tangent shared, so the curves leave and arrive along one direction and the corner disappears._

Here we can see the shared tangent.

### Worked example

We can see a $C^1$ example with the following:

$$c_1(t) = \begin{bmatrix} t \\ t^2 \end{bmatrix} \qquad c_2(t) = \begin{bmatrix} 1 + t \\ 1 + 2t + 3t^2 \end{bmatrix}$$

We can check their positions with the following:

$$c_1(1) = (1, 1) \quad \text{and} \quad c_2(0) = (1, 1)$$

So we can say that they meet. For the derivatives we have:

$$c_1'(t) = \begin{bmatrix} 1 \\ 2t \end{bmatrix} \qquad c_2'(t) = \begin{bmatrix} 1 \\ 2 + 6t \end{bmatrix}$$

So:

$$c_1'(1) = (1, 2) \quad \text{and} \quad c_2'(0) = (1, 2)$$

We can now explore second derivatives, which are:

$$c_1''(t) = \begin{bmatrix} 0 \\ 2 \end{bmatrix} \qquad c_2''(t) = \begin{bmatrix} 0 \\ 6 \end{bmatrix}$$

We can see that these are constants and they will not match anywhere, let alone the join. This means that our curves have a matching tangent at the join but are still changing direction at different rates immediately after the join. If they did match then we would have stronger smoothness conditions that can be useful for shape quality.

### Parameter ranges aren't always $[0, 1]$

With B-splines the range is not always $[0, 1]$. For example, consider the following knot vector:

$$u = [0, 0, 0, 0, 1, 2, 3, 3, 3, 3], \qquad t \in [0, 3]$$

So for a polynomial curve linking to a B-spline we have the following:

$$c_1 : [0, 1] \to \mathbb{R}^3 \quad \text{and} \quad c_2 : [2, 5] \to \mathbb{R}^3 \;\Rightarrow\; c_1(1) = c_2(2)$$

> [!note] Correction
> The handwritten version had $c_2 : [2, 5] \to \mathbb{R}^2$. Both curves have to live in the same space for the join to make sense, so I've made it $\mathbb{R}^3$ to match $c_1$. The point of the example — that the join condition is "end parameter of $c_1$ equals start parameter of $c_2$", whatever those parameter values are — is unchanged.

## Geometric continuity

For geometric continuity we only care about the **direction** of the tangent, as opposed to parametric continuity where we care about the direction **and magnitude**. Geometric continuity ($G^1$) is defined by the following:

$$c_1'(1) = \lambda \, c_2'(0), \qquad \lambda > 0$$

So let's say that $c_1'(1) = (4, 2)$ and $c_2'(0) = (2, 1)$. We know they do not match parametrically. But if

$$\lambda = 2 \;\Rightarrow\; c_1'(1) = \lambda \, c_2'(0) \;\Rightarrow\; (4, 2) = 2 \, (2, 1)$$

### Why should we care about geometric continuity?

A good way to think about this is two different cars moving along a track. They are driving along the same road and direction but one is double the speed of the other. The direction of the cars is the same but the speed differs. If we just care about tangent continuity then we should nullify the speed to see if their directions are the same.

### Second derivative ($G^2$)

For the second derivative we have the following:

$$c_1''(1) = \lambda^2 \, c_2''(0) + \mu \, c_2'(0)$$

To demonstrate this, we can go back to our numerical example where $\lambda = 2$. Now suppose their second derivatives are:

$$c_1''(1) = (6, 5) \qquad c_2''(0) = (1, 1)$$

But then, even though they're not equal, we can still test $G^2$ continuity with the following:

$$(6, 5) = 2^2 \, (1, 1) + \mu \, (2, 1) \;\Rightarrow\; (6, 5) = (4 + 2\mu, \; 4 + \mu)$$

Therefore: $\mu = 1$ (both components agree).

> [!note] Correction
> The handwritten version had $c_1''(1) = (6, 4)$ and used $c_2'(0) = (2, 0)$ in the $\mu$ term. But $c_2'(0)$ was already fixed as $(2, 1)$ in the $\lambda$ example above, and with that value $(6, 4)$ gives $2\mu = 2$ from the first component but $\mu = 0$ from the second — so it's _not_ $G^2$. I've changed $c_1''(1)$ to $(6, 5)$ so that $\mu = 1$ comes out consistently from both components and the example demonstrates what it's meant to. The key idea is the same: $\mu$ has to be a single scalar that satisfies every component at once.

Now that we have continuity we can cover curvature.

## Curvature

In CAD, curvature is important because two curves can meet at the same point, and point in the same direction, but still have a noticeable change in how sharply they bend. This is the difference between $G^1$ (tangent continuity) and $G^2$ (curvature continuity).

A simple measurement can be of a circle with the following:

$$\kappa = \frac{1}{R} \qquad \text{where } R \text{ is the radius}$$

Curvature is the measurement of how sharply a curve bends at a particular point. For example, the curvature of a circle with a radius of 10 has a curvature of 0.1; if the radius is 2 it is 0.5; and a straight line has a curvature of zero. Curvature is pretty important for reflection:

- $G^0$: reflections appear broken because the surfaces meet at an angle
- $G^1$: reflections connect, but their direction can change abruptly across the join
- $G^2$: reflections flow smoothly because the curvature matches

We can measure the curvature of a general parametric curve with the following:

$$\kappa(t) = \frac{\lVert c'(t) \times c''(t) \rVert}{\lVert c'(t) \rVert^3}$$

> [!note]
> This line was cut off at the bottom of page 3 in the photo — the formula above is the standard one that the page-4 bullets are describing.

Here:

- $c'(t)$: tells us the tangent direction and parameter speed
- $c''(t)$: tells us how that tangent vector is changing
- The cross product extracts the component associated with bending rather than simply speeding up or slowing down

The division by $\lVert c'(t) \rVert^3$ removes the dependence on parameter speed.

---

## Connections

- The other half of the pair: [[Piecewise polynomials and rational maps]] is what the pieces _are_, this note is how they are allowed to meet. A breakpoint $x_k$ there is a join here, so every $C^k$ and $G^k$ condition above is a condition on $f_{k-1}$ and $f_k$ at that one parameter value. Two consequences worth carrying across: a piecewise definition on half-open intervals guarantees nothing about smoothness — its own worked example jumps at the breakpoint, so it is not even $C^0$ — and for a rational piece $\vec P(t)/Q(t)$ the tangent $c'(t)$ needs the quotient rule, so the $\lambda$ relating two tangents absorbs the denominators as well as the parameter speed.
- Part of [[CAD Geometry]], and the note [[Bezier Curves]] has been pointing at: its tangent formulas $\mathbf{B}'(0) = n(\mathbf{P}_1 - \mathbf{P}_0)$ and $\mathbf{B}'(1) = n(\mathbf{P}_n - \mathbf{P}_{n-1})$ are $c'(0)$ and $c'(1)$ for a Bézier segment, so "make the last two control points of one segment collinear with the first two of the next" is $c_1'(1) = \lambda\,c_2'(0)$ with $\lambda > 0$ — the $G^1$ condition in control-point form, and $\lambda = 1$ recovers $C^1$.
- The knot vector $u = [0,0,0,0,1,2,3,3,3,3]$ is the one from [[B-Splines and NURBS]], where the parameter domain is $[0,3]$ rather than $[0,1]$ — which is why the join condition has to be stated as "end parameter of $c_1$ equals start parameter of $c_2$". That note's open question about what knot multiplicity does to continuity is this note's conditions read off the basis: each repeat of a knot drops the guaranteed continuity by one.
- $\lambda$ and $\mu$ are not free parameters, they are the derivatives of a reparametrisation. Writing $c_1 = c_2 \circ \varphi$ and differentiating with [[chain-rule|the chain rule]] gives $c_1' = \varphi' c_2'$, so $\lambda = \varphi'$; differentiating again with [[product rule|the product rule]] gives $c_1'' = (\varphi')^2 c_2'' + \varphi'' c_2'$, which is exactly the $G^2$ condition with $\mu = \varphi''$. The two cars on one track are $\varphi$ — same road, different clock.
- $\lVert c' \times c'' \rVert$ is the area of the parallelogram spanned by velocity and acceleration — [[Vector Product]], computed by the minors of [[Determinant (cofactor) form of the cross product]]. It isolates bending because the part of $c''$ parallel to $c'$ is pure speeding up, and parallel vectors have zero cross product. Dividing by $\lVert c' \rVert^3$ strips the parameter speed out, the same normalise-to-direction move as [[Unit vectors]], which is also all that $\lambda > 0$ is doing in $G^1$.
- The derivatives are componentwise on a vector-valued map, as set out in [[Basic Operations]], and the limit underneath them is [[lim-zero-example]]. The curve lives in [[Euclidean space]], where $\lVert \cdot \rVert$ is the straight-line norm.
- The two reference curvatures are elsewhere in the vault: $\kappa = 1/R$ for a circle is the parametrisation of [[Trigonometry]] — radius $R$, and $\kappa \to 0$ as $R \to \infty$ — and $\kappa = 0$ for the straight lines of [[Lines]], whose second derivative vanishes identically.
- The mesh version of the same ladder: a triangulated [[Cylinder]] or [[Sphere]] is only $G^0$ across its edges no matter how fine the tessellation, because the facets genuinely meet at an angle. Interpolated vertex normals fake $G^1$ for shading, which is why a silhouette still shows the facets while the interior looks smooth — the reflection test in this note is exactly what that trick is hiding.
- Part of [[Home]].

## Open

- [ ] $\kappa(t)$ evaluated for the worked example above, to show $G^1$ but not $G^2$ numerically rather than by inspecting $c''$.
- [ ] Curvature combs: plotting $\kappa$ along the curve as the standard way of spotting a $G^1$-but-not-$G^2$ join by eye.
- [ ] Surface continuity across patches, where the condition has to hold along a whole boundary curve rather than at one point.
- [ ] The Frenet frame — normal, binormal and torsion — with curvature as the first of the two invariants of a space curve.
- [ ] Arc-length parametrisation, where $\lVert c' \rVert = 1$ makes $C^k$ and $G^k$ coincide and $\kappa = \lVert c'' \rVert$.
