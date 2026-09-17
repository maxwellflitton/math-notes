---
tags:
  - moc
area: cad
---
# CAD Geometry

The curves and surfaces this vault is aimed at. Part of [[Home]].

## Notes
- [[Bezier Curves]] — control points blended by Bernstein polynomials.
- [[B-Splines and NURBS]] — the generalisation: local control, arbitrary segment counts, rational weights.

## Mesh algorithms
- [[Cylinder]] — building a triangle mesh for a cylinder: the vertex and face arrays, edges as derived data ($E = \partial F$), and the closed form for both.

## Projection
- [[Ortho derivation]] — orthographic projection from first principles: splitting a vector along and across the view, the camera axes, the view matrix and the projection matrix.
- [[Ortho Application]] — the engine's matrices assembled: $PV$, the map to pixels, row form and running it backwards for picking.

## What it builds on
- **Space:** control points and the convex hull live in [[Euclidean space]]; the control polygon is a chain of [[Lines]].
- **Calculus:** endpoint tangents $\mathbf{B}'(0) = n(\mathbf{P}_1 - \mathbf{P}_0)$ come straight from differentiating the polynomial — see [[Calculus]] and [[chain-rule]].
- **Algebra:** the Bernstein weights are a binomial expansion — see [[exp-algebria-rules]] for the index laws.

## Open
- [ ] Parametric curves and surfaces in general.
- [ ] Continuity and smoothness ($C^n$ vs $G^n$) at segment joins.
- [ ] Transformations (translate/rotate/scale as matrices) — the rotation matrix already appears in [[compound angle formula]].
- [ ] Surfaces as tensor products of two curves.

## Related
- [[Linear Algebra]], [[Calculus]], [[Home]]
