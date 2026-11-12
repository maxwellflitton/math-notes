---
tags: [maths, cad, geometry, bezier]
aliases: [Bezier curves, De Casteljau, Bernstein polynomials]
---

# Bézier Curves

## 1. Starting from a line (lerp)

We can define a line between two points with the following:


![A line from $P_0$ to $P_1$ with the point p marked t of the way along](_attachments/bezier-01-lerp-line.png)

$t$ is the *fraction* of the way from $P_0$ to $P_1$: $t=0$ gives $P_0$, $t=1$ gives $P_1$, $t=0.5$ the midpoint. This is the `lerp` used throughout the rest of the note, and the two forms are the same thing rearranged:

$$\operatorname{lerp}(a,b,t) = a + (b-a)\,t = (1-t)\,a + t\,b$$

With this, we can grow our example to two lines, moving a point along each by the same $t$:

![Two joined line segments, each with a point marked at the same fraction t along it](_attachments/bezier-02-two-lines.png)

We can then connect the two points with a line segment, giving the following:

![The two t-points from the previous diagram joined by a new segment](_attachments/bezier-03-connect-points.png)

Then we add another point on that segment, again at the same $t$, and get the following curve at different stages of $t$:

![The same construction at t = 0.2, 0.5 and 0.8, and the full quadratic curve traced as t runs from 0 to 1](_attachments/bezier-04-quadratic-stages.png)

That new point isn't in the *middle* of the segment: it sits at fraction $t$ along it, the same $t$ used on the outer lines, and is only the middle when $t = 0.5$.

The point on the middle segment traces out the curve on the far right as $t$ moves from $0 \to 1$. This path is a **quadratic Bézier curve**.

Written out in one go, with control points $P_0, P_1, P_2$, that is

$$B(t) = (1-t)^2 P_0 + 2t(1-t)\,P_1 + t^2 P_2$$

## 2. Cubic Bézier curve

We can go further with a **cubic Bézier curve**. It uses 4 control points and one more level of lerps:

![De Casteljau's algorithm on four control points: lerps a, b, c, then d, e, then pos — shown at t = 0.5 and on an S-shaped control polygon at t = 0.3](_attachments/bezier-05-cubic-de-casteljau.png)

$$
\begin{aligned}
a &= \operatorname{lerp}(P_1, P_2, t) \\
b &= \operatorname{lerp}(P_2, P_3, t) \\
c &= \operatorname{lerp}(P_3, P_4, t) \\
d &= \operatorname{lerp}(a, b, t) \\
e &= \operatorname{lerp}(b, c, t) \\
\text{pos} &= \operatorname{lerp}(d, e, t)
\end{aligned}
$$

where

$$
\operatorname{lerp}(a, b, t) = a + (b-a)\,t
$$

Calculating positions this way is known as **De Casteljau's algorithm**.

## 3. Expanding the lerps

Now let's write out all the lerps manually:

$$
\begin{aligned}
a &= (1-t)P_1 + tP_2 & \qquad d &= (1-t)\,a + t\,b \\
b &= (1-t)P_2 + tP_3 & \qquad e &= (1-t)\,b + t\,c \\
c &= (1-t)P_3 + tP_4 & \qquad \text{pos} &= (1-t)\,d + t\,e
\end{aligned}
$$

Now we can expand the pos equation by substituting in $d$ and $e$, then substituting in $a$, $b$ and $c$, and expanding out to get the following:

$$
P(t) = -P_1t^3 + 3P_1t^2 - 3P_1t + P_1 + 3P_2t^3 - 6P_2t^2 + 3P_2t - 3P_3t^3 + 3P_3t^2 + P_4t^3
$$

We can now group the points in the equation with the following:

$$
P(t) = P_1\left(-t^3 + 3t^2 - 3t + 1\right) + P_2\left(3t^3 - 6t^2 + 3t\right) + P_3\left(3t^2 - 3t^3\right) + P_4\left(t^3\right)
$$

## 4. Weights: the Bernstein polynomial form

We now have 4 weights, as seen below:

![Left: the four weights at t = 0.4 as arrows pulling pos towards each control point. Right: the Bernstein basis curves over t, summing to 1](_attachments/bezier-06-bernstein-weights.png)

$$
\begin{aligned}
t = 0 &\implies w_1 = 1 \implies \text{pos} = P_1 \\
t = 1 &\implies w_4 = 1 \implies \text{pos} = P_4
\end{aligned}
$$

All these weights (as vectors pointing to the control points) sum to one. This follows the exact same curve we got with lerps. This is called the **Bernstein polynomial form** of Bézier curves.

The weight brackets factor neatly:

$$P(t) = (1-t)^3P_1 + 3t(1-t)^2P_2 + 3t^2(1-t)P_3 + t^3P_4 = \sum_{i=0}^{3}\binom{3}{i}(1-t)^{3-i}\,t^{i}\,P_{i+1}$$

They sum to one because they are the terms of $\big((1-t)+t\big)^3 = 1^3 = 1$. Every point on the curve is therefore a weighted average of the control points, which is why the curve always stays inside the control polygon — the convex hull property.

## 5. Derivatives

The points are constants, so we can take derivatives with respect to $t$, giving the following:

$$
P'(t) = P_1\left(6t - 3t^2 - 3\right) + P_2\left(9t^2 - 12t + 3\right) + P_3\left(6t - 9t^2\right) + P_4\left(3t^2\right)
$$

This is the **tangent** (velocity) of the curve. We can also get the **normal** vector by rotating it 90°:

![Tangent T = P'(t) along the curve, normal N as T rotated 90 degrees, and acceleration A = P''(t) pointing the opposite way, into the turn](_attachments/bezier-07-tangent-normal-accel.png)

$$
T = (T_x,\, T_y) \quad\Longrightarrow\quad N = (-T_y,\, T_x)
$$

If we differentiate again, we get the **acceleration** of the curve:

$$
P''(t) = P_1\left(6 - 6t\right) + P_2\left(18t - 12\right) + P_3\left(6 - 18t\right) + P_4\left(6t\right)
$$

The tangent (velocity) can be thought of as a quadratic Bézier curve, and the acceleration as a linear Bézier curve (just a lerp).

Regroup the derivatives in Bernstein form and the differences between control points become the new control points:

$$P'(t) = 3\Big[(1-t)^2(P_2-P_1) + 2t(1-t)(P_3-P_2) + t^2(P_4-P_3)\Big]$$

$$P''(t) = 6\Big[(1-t)(P_3 - 2P_2 + P_1) + t\,(P_4 - 2P_3 + P_2)\Big] = 6\operatorname{lerp}\big(P_3-2P_2+P_1,\; P_4-2P_3+P_2,\; t\big)$$

Each derivative drops the degree by one: cubic → quadratic → linear → constant.

The rotated normal $N$ is fixed by which way you rotate — counter-clockwise here — so it can end up on either side of the curve. The acceleration, on the other hand, always bends towards the inside of the turn: in the diagram $N$ points up while $A$ points down, towards the centre of curvature. The sign of the curvature, covered below, is what tells you which side that is.

We can do one more derivative and get the **"jerk"**, which is just a constant:

$$
P'''(t) = P_1(-6) + P_2(18) + P_3(-18) + P_4(6)
$$

## 6. Curvature

Finally, we can calculate curvature with the following:

$$
\kappa = \frac{\det\!\left(P',\, P''\right)}{\lVert P' \rVert^{3}} = \frac{x'y'' - y'x''}{\left(x'^2 + y'^2\right)^{3/2}} \qquad \left[\text{length}^{-1},\ \text{e.g. } \mathrm{m}^{-1}\right]
$$

This is **signed** curvature: $\kappa > 0$ means the curve turns left (counter-clockwise), $\kappa < 0$ means it turns right, and $\kappa = 0$ means it is locally straight — an inflection point. The units are 1/length, so coordinates in metres give a $\kappa$ in $\mathrm{m}^{-1}$.

In 3D the determinant becomes a cross product, and the sign goes with it:

$$\kappa = \frac{\lVert P' \times P'' \rVert}{\lVert P' \rVert^3}$$

## 7. Radius of curvature

The radius of curvature can be found with $r = \kappa^{-1}$, and we have the following:

![The osculating circle touching the curve at P(t), with radius r = 1/kappa drawn from the curve to its centre](_attachments/bezier-08-radius-of-curvature.png)

The circle of radius $r$ that touches the curve at $P(t)$ is the **osculating circle**, the circle that best matches the curve at that point. Using the unit normal $\hat N$ (the unit tangent rotated 90° counter-clockwise) and the signed $\kappa$, its centre is

$$C = P(t) + \frac{1}{\kappa}\,\hat N$$

When $\kappa < 0$ the $\tfrac{1}{\kappa}$ is negative, which flips the centre to the other side automatically. Tight bends mean a large $\kappa$ and a small circle; near-straight sections mean $\kappa \to 0$ and $r \to \infty$.

## Connections

- Part of [[CAD Geometry]], and the first curve in it. [[B-Splines and NURBS]] is the sequel in two steps: chain several of these together with continuity conditions at the joins, then let each control point carry a weight so the map becomes rational and circles become exact. A Bézier curve is that construction with no interior knots and all weights equal.
- The join conditions themselves are [[Curve continuity]]. Section 5 is what those conditions constrain: regrouped in Bernstein form the derivative's control points are the *differences* $P_2 - P_1, P_3 - P_2, P_4 - P_3$, so $P'(0) = 3(P_2 - P_1)$ and $P'(1) = 3(P_4 - P_3)$ — the end tangent is three times the first (or last) leg of the control polygon. $C^1$ at a join means those legs match in length and direction, $G^1$ only in direction.
- The lerp of §1 is the parametric line of [[Lines]], and the degree-1 Bézier curve: $\mathbf{r}(t) = (1-t)\mathbf{p} + t\mathbf{q}$ is the same formula read as a curve rather than a line ([[Basic Operations]]). Everything above is that one operation nested — which is why the whole construction lives in [[Euclidean space]] and needs nothing but addition and scaling.
- Weights summing to one is what makes this an *affine* combination, and that is not a technicality: a weighted average of points is only meaningful when the weights sum to $1$, which is exactly the distinction [[Displacement vs Position Vectors]] draws. It also gives affine invariance — transform the control points and you have transformed the curve, so the camera of [[Ortho Application]] never has to touch the curve itself, only four points ([[Matrix Overview]]).
- De Casteljau is a recurrence with a base case and the Bernstein form is its closed form, the trade [[basic recursion for sum loop]] is about: nested lerps evaluated step by step, against one polynomial evaluated directly. Same curve, different cost profile — the recursion is numerically steadier and splits the curve for free, the polynomial is faster per point. Picking $n$ values of $t$ to turn the curve into geometry is the accuracy-versus-cost choice of [[Cylinder]].
- The weights are the binomial expansion of $\big((1-t) + t\big)^3$, so the $\binom{3}{i}$ and the index laws behind $t^i(1-t)^{3-i}$ are [[exp-algebria-rules]], and the parameter domain $t \in [0,1]$ is the interval notation of [[sets]]. The convex hull property follows from the weights being non-negative and summing to one — and computing that hull is the monotone chain of [[Monotonic Stack]].
- $N = (-T_y, T_x)$ is the rotation matrix $R(90°)$ applied to the tangent — [[Matrix Overview]]. Normalising it to $\hat N$ for the osculating-circle centre is [[Unit vectors]], and testing whether two curves meet tangentially is a dot product on their end tangents ([[Scalar Product]]).
- The numerator of $\kappa$, $x'y'' - y'x''$, is a $2 \times 2$ determinant: the signed area of the parallelogram spanned by velocity and acceleration, which is why its sign says which way the curve turns — [[Determinant (cofactor) form of the cross product]]. The 3D form $\lVert P' \times P'' \rVert$ is the same quantity as a [[Vector Product]], with the sign giving way to a magnitude because "left" and "right" stop meaning anything in space.
- Differentiating $P(t)$ is ordinary polynomial calculus ([[Calculus]]) — the terms are products like $t^2(1-t)$, so the regrouping in §5 is the [[product rule]] applied and tidied, and the tangent as a limit of chords is [[lim-zero-example]]. Each derivative drops the degree by one, which is why velocity is a quadratic Bézier and acceleration a linear one.
- Polynomials piecewise in $t$, and what happens when you divide one by another, are set out in [[Piecewise polynomials and rational maps]] — the background for why a cubic cannot draw an exact circle and a rational map can.
- Interpolating rotations rather than positions needs the sphere instead of the straight line: slerp in [[quaternions]] is this note's $t$ running along a great arc.

## Open

- [ ] Splitting a curve at $t$: de Casteljau already computes both halves' control points, which is the basis of adaptive tessellation and of trimming in CAD.
- [ ] Degree elevation — rewriting a cubic as a quartic with the same shape — and why it is needed to join curves of different degrees.
- [ ] Arc length: $\int_0^1 \lVert P'(t) \rVert \, dt$ has no closed form for a cubic, so what reparameterising by arc length actually costs at runtime ([[Calculus]], [[Direct Integration]]).
- [ ] The Frenet frame $(T, N, B)$ in 3D, with the binormal from [[Vector Product]], and curvature plus torsion as the full description of a space curve.
- [ ] Offset curves: the offset of a cubic Bézier is not a Bézier, which is why CAD tool paths approximate — and where [[Curve continuity]]'s $G^2$ matters for the finish.
- [ ] Evaluating de Casteljau per vertex against precomputing the Bernstein coefficients, benchmarked the way [[Cylinder]] argues its closed form.
