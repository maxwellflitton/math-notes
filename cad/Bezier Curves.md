---
tags:
  - concept
area: cad
status: learning
---
# Bezier Curves

> [!summary] In a sentence
> A Bézier curve is a smooth curve defined by a handful of control points, blended together by Bernstein polynomials.

## Intuition
You don't draw the curve directly — you place **control points** and the curve is pulled
toward them. The first and last control points are the endpoints; the inner ones act like
magnets shaping the path. This is the workhorse of vector graphics and the building block for
[[B-Splines and NURBS]].

## Definition
A degree-$n$ Bézier curve with control points $\mathbf{P}_0,\dots,\mathbf{P}_n$ is

$$
\mathbf{B}(t) = \sum_{i=0}^{n} \binom{n}{i}\, (1-t)^{\,n-i}\, t^{\,i}\, \mathbf{P}_i,
\qquad t \in [0,1].
$$


The weights $b_{i,n}(t) = \binom{n}{i}(1-t)^{n-i}t^i$ are the **Bernstein polynomials**;
they are non-negative and sum to 1, so the curve is a moving weighted average of the controls.

## Key results / formulas
- **Cubic** ($n=3$) is the common case: $\mathbf{B}(t)=(1-t)^3\mathbf{P}_0 + 3(1-t)^2 t\,\mathbf{P}_1 + 3(1-t)t^2\mathbf{P}_2 + t^3\mathbf{P}_3.$
- **Endpoints:** $\mathbf{B}(0)=\mathbf{P}_0$, $\mathbf{B}(1)=\mathbf{P}_n$.
- **Tangents:** $\mathbf{B}'(0)=n(\mathbf{P}_1-\mathbf{P}_0)$, $\mathbf{B}'(1)=n(\mathbf{P}_n-\mathbf{P}_{n-1})$ — control the leaving/arriving direction. Basis for [[Continuity and Smoothness]].
- **Convex hull:** the curve stays inside the polygon of its control points (because weights are a partition of unity).
- **de Casteljau's algorithm:** evaluate by repeated linear interpolation — numerically stable, and also splits the curve.

## Worked example
Cubic tangent at the start. With controls $\mathbf{P}_0,\dots,\mathbf{P}_3$ and $n=3$:

$$
\mathbf{B}'(0) = 3(\mathbf{P}_1 - \mathbf{P}_0).
$$

> [!check] Interpretation
> The curve leaves $\mathbf{P}_0$ heading straight at $\mathbf{P}_1$. To join two cubics with smooth $G^1$ continuity, make $\mathbf{P}_0,\mathbf{P}_1$ of the second segment collinear with the last two of the first.

## Connections
- Part of [[CAD Geometry]]. Built from [[Parametric Curves and Surfaces]]; generalized by [[B-Splines and NURBS]].
- Smooth joins use [[Continuity and Smoothness]].
- Tangents/derivatives draw on [[Calculus]] and [[chain-rule|the chain rule]], and later on [[Vector Calculus]] and [[Partial Derivatives]].
- Control points, the convex hull and the distance metric all live in [[Euclidean space]]; the control polygon is a chain of [[Lines]], and the degree-1 curve $\mathbf{B}(t) = (1-t)\mathbf{P}_0 + t\mathbf{P}_1$ *is* a line segment — de Casteljau just repeats that interpolation.
- The parameter domain $t \in [0,1]$ in set-builder terms: [[sets]]. Binomial/index algebra for the Bernstein weights: [[exp-algebria-rules]].

## Open questions / TODO
- [ ] Derive de Casteljau and show it equals the Bernstein form.
- [ ] Extend to a Bézier surface (tensor product of two curves).

## References
- 
