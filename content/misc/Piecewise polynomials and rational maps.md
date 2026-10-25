---
tags:
  - cad
  - maths
  - curves
  - polynomials
  - rational-maps
date: 2026-10-25
---

# Piecewise polynomials and rational maps

## Piecewise polynomial

A piecewise polynomial is a function that is defined by a different polynomial on each of several adjacent intervals and then glued together. Formally, we pick breakpoints like the following:

$$x_0 < x_1 < \dots < x_n$$

A piecewise polynomial of degree $p$ is $f(x) = f_k(x)$ for $x \in [x_k, x_{k+1})$.

For example, let's pick three random functions below:

$$f_0(x) = x^2 \qquad f_1(x) = 2x + 1 \qquad f_2(x) = x^3$$

This gives the following:

$$
f(x) =
\begin{cases}
x^2 & 0 \le x < 2 \\
2x + 1 & 2 \le x < 4 \\
x^3 & 4 \le x < 6
\end{cases}
$$

Here we can see that $k$ is simply an index that identifies which segment you're working on.

However, polynomials can fail to do things like draw circles, so we can use polynomials to construct rational maps.

## Rational maps

This is where we describe a curve with one polynomial divided by another polynomial. This can be shown with the following:

$$R(t) = \frac{t^2}{1 + t^2} \;\Rightarrow\; R(t) = \frac{P(t)}{Q(t)}$$

where both numerator and denominator are polynomials, which produces a rational function. In CAD it can take the following:

$$
c(t) =
\begin{bmatrix}
\dfrac{P_x(t)}{Q(t)} \\[1.2em]
\dfrac{P_y(t)}{Q(t)} \\[1.2em]
\dfrac{P_z(t)}{Q(t)}
\end{bmatrix}
\quad \text{or} \quad
c(t) = \frac{\vec{P}(t)}{Q(t)}
\;\Rightarrow\; c : [a, b] \to \mathbb{R}^3
$$

### The unit circle as a rational map

We can see this in action by drawing the unit circle using rational functions. We remember that the radius of the unit circle is:

$$x^2 + y^2 = 1$$

with the parametric representation of the following:

$$c(\theta) = \begin{bmatrix} \cos\theta \\ \sin\theta \end{bmatrix}$$

This generates a perfect circle, but we can define the unit circle with the following:

$$
c(t) =
\begin{bmatrix}
\dfrac{1 - t^2}{1 + t^2} \\[1.2em]
\dfrac{2t}{1 + t^2}
\end{bmatrix}
\;\Rightarrow\; c(0) = (1, 0) \;\Rightarrow\; c(1) = (0, 1)
$$

We can also verify the radius with the following:

$$\left( \frac{1 - t^2}{1 + t^2} \right)^2 + \left( \frac{2t}{1 + t^2} \right)^2 = 1$$

> [!note] Why this works
> This is the Weierstrass substitution $t = \tan(\theta/2)$ — the half-angle formulas give $\cos\theta = \frac{1 - t^2}{1 + t^2}$ and $\sin\theta = \frac{2t}{1 + t^2}$. So the rational map _is_ the trig map, just reparametrised. One consequence worth remembering: $t \in [0, 1]$ only sweeps the quarter-circle from $(1, 0)$ to $(0, 1)$, and no finite $t$ ever reaches $(-1, 0)$, which is why CAD uses several rational pieces (a rational B-spline / NURBS) to close a full circle rather than one.

---

## Connections

- Part of [[CAD Geometry]]. [[Curve continuity]] opens by saying CAD uses "piecewise polynomial or rational maps" — this note is what those two families are, and that note is how the pieces are allowed to meet. A breakpoint $x_k$ here is a join there, so $C^k$ and $G^k$ are conditions on $f_{k-1}$ and $f_k$ at the same $x_k$.
- Worth noticing about the three-piece example: $f_0(2^-) = 4$ but $f_1(2) = 5$, so it jumps at the breakpoint. That is legal for a piecewise function — the definition only asks that each piece own its half-open interval — but it is not even $C^0$, which is exactly the condition [[Curve continuity]] starts from. "Piecewise polynomial" and "smooth" are independent properties, and CAD wants both.
- The rational form is the R in NURBS: with a weight $w_i$ per control point, $c(t) = \frac{\sum_i w_i \mathbf{P}_i N_i(t)}{\sum_i w_i N_i(t)}$, which is $\vec P(t)/Q(t)$ with the B-spline basis functions supplying both sums — [[B-Splines and NURBS]], whose open question about the circle being exact as a NURBS and impossible as a polynomial Bézier is half-answered by the worked example here. A [[Bezier Curves|Bézier curve]] is the case $Q(t) \equiv 1$, all weights equal.
- Why no polynomial can do it: if $x(t)^2 + y(t)^2 = 1$ with $x, y$ real polynomials, the leading terms of the two squares cannot cancel (both have positive leading coefficients), so $2\max(\deg x, \deg y) = \deg 1 = 0$ and both must be constant. The circle is rational but not polynomial, which is the whole reason the division is worth its cost.
- The substitution $t = \tan(\theta/2)$ comes straight out of the half-angle identities in [[compound angle formula]], and the curve being parametrised is the unit circle of [[Trigonometry]]. The same substitution appears in the other direction in [[by-substitution]] and the tables of [[trig|trig integration]]: there it is used to turn a trig integrand into a rational one, here to turn a trig parametrisation into a rational map. One change of variable, two jobs.
- The half-open intervals $[x_k, x_{k+1})$ are a partition in the sense of [[sets]]: every $x$ in range lies in exactly one piece, which is what makes $f$ well defined at the breakpoints with no tie to break. Degree $1$ pieces make this the chain of [[Lines]] that a control polygon already is.
- $c : [a,b] \to \mathbb{R}^3$ lands in [[Euclidean space]], and the component form of $c(t)$ is the column vector of [[Basic Operations]] with each component a scalar function of $t$.
- Part of [[Home]].

## Open

- [ ] The weighted NURBS form written out, showing how $w_i$ pulls the curve toward $\mathbf{P}_i$ and what $w_i = 0$ and $w_i \to \infty$ do.
- [ ] The full circle as several rational pieces: which arcs, which weights, and why the standard construction uses quarter or third arcs.
- [ ] Continuity at a breakpoint in terms of degree: how many conditions a degree-$p$ piece can satisfy, and the $C^{p-1}$ ceiling that knot multiplicity eats into.
- [ ] Derivative of a rational map by the quotient rule, and why the denominator makes tangent calculations messier than in the polynomial case.
