---
tags:
  - moc
area: linear-algebra
---
# Linear Algebra

Space, distance and the objects living in it. Part of [[Home]].

## Notes
- [[Euclidean space]] — what "Euclidean" actually claims: straight-line distance, Pythagoras, parallel lines never meeting.
- [[Lines]] — standard, slope-intercept, point-slope and two-point forms, and where each one breaks.

## Vectors
- [[Basic Operations]] — component form, magnitude, scaling, addition, and componentwise calculus.
- [[Displacement vs Position Vectors]] — the triangle law, and why $\overrightarrow{OQ}$ and $\overrightarrow{PQ}$ are different objects.
- [[Unit vectors]] — normalising to direction cosines, recovering the third angle, and what it costs to store a direction.
- [[Scalar Product]] — the dot product, the angle formula, orthonormal bases and projection.
- [[Vector Product]] — the cross product, spanned area, the normal $\hat{n}$ and anticommutativity.
- [[Determinant (cofactor) form of the cross product]] — the $\hat{\imath}\,\hat{\jmath}\,\hat{k}$ mnemonic expanded, minors as shadow areas, and determinant vs dot product.
- [[Volumes and the scalar triple product]] — cross then dot for the volume of a parallelepiped, the cyclic identity, and the determinant as a volume scaling factor.

## Operators
- [[eigen]] — the vectors and functions a linear operator only scales, and what eigen-decomposition buys you.

## Threads
- Both notes are written in set-builder notation — read [[sets]] first if `{ (x,y) | ax + by = c }` looks opaque.
- Distance in [[Euclidean space]] is the metric assumed by every curve in [[CAD Geometry]]; control points and the convex hull of [[Bezier Curves]] live in it.
- Rotations of the plane are linear maps on [[Euclidean space]] — that fact is what proves the [[compound angle formula|compound angle formulae]] for all angles.
- [[Scalar Product]], [[Vector Product]] and [[eigen]] come together in the camera matrices of [[Ortho derivation]] and [[Ortho Application]].
- The slope $m$ in [[Lines]] is the constant-rate special case of the derivative — see [[lim-zero-example]].
- Area in 2D, volume in 3D: the $2\times2$ determinant of [[Determinant (cofactor) form of the cross product]] and the $3\times3$ one of [[Volumes and the scalar triple product]] are one idea a dimension apart, and the bridge into matrices as transformations that scale volume.
- Matrices over Booleans, with OR as addition and AND as multiplication: the transitive closure $A^* = I \lor A \lor A^2 \lor \cdots$ in [[Bitset Auth]].

## Open
- [ ] Dot/cross products, matrices and transformations.
- [ ] Diagonalisation and the characteristic polynomial, following on from [[eigen]].

## Related
- [[CAD Geometry]] — the main consumer of this area.
- [[Trigonometry]], [[Foundations]]
