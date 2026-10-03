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

## Matrices
- [[Matrix algebra]] — matrices as linear systems, $A = [a_{ij}]$ notation, the inner-dimension rule, $c_{ij} = \sum_k a_{ik} b_{kj}$ worked entry by entry, and the outer product.
- [[Matrix Overview]] — order and the inner-dimension rule, composing transformations, non-commutativity, and inversion down to the $2 \times 2$ formula.
- [[Determinants and inverses for matrices]] — the determinant as signed area and volume, cofactor expansion, and the adjugate inverse worked at $3 \times 3$.

## Operators
- [[eigen]] — the vectors and functions a linear operator only scales, and what eigen-decomposition buys you.

## Threads
- Both notes are written in set-builder notation — read [[sets]] first if `{ (x,y) | ax + by = c }` looks opaque.
- Distance in [[Euclidean space]] is the metric assumed by every curve in [[CAD Geometry]]; control points and the convex hull of [[Bezier Curves]] live in it.
- Rotations of the plane are linear maps on [[Euclidean space]] — that fact is what proves the [[compound angle formula|compound angle formulae]] for all angles.
- [[Scalar Product]], [[Vector Product]] and [[eigen]] come together in the camera matrices of [[Ortho derivation]] and [[Ortho Application]].
- The slope $m$ in [[Lines]] is the constant-rate special case of the derivative — see [[lim-zero-example]].
- Area in 2D, volume in 3D: the $2\times2$ determinant of [[Determinant (cofactor) form of the cross product]] and the $3\times3$ one of [[Volumes and the scalar triple product]] are one idea a dimension apart, and the bridge into [[Matrix Overview|matrices]] as transformations that scale volume — that same $ad - bc$ is the denominator of the inverse.
- The determinant is where the vector notes and the matrix notes meet: the $ad - bc$ of [[Determinant (cofactor) form of the cross product]] and the triple product of [[Volumes and the scalar triple product]] are the same number that [[Determinants and inverses for matrices]] divides by to invert a matrix.
- Rotations compose: $R(\alpha)R(\beta) = R(\alpha + \beta)$ in [[Matrix Overview]] is the [[compound angle formula|compound angle formulae]] read as one matrix product.
- The camera of [[Ortho derivation]] and [[Ortho Application]] is [[Matrix Overview]] applied: successive transformations multiplied into one $PV$, order-sensitive, then inverted for picking.
- Matrices over Booleans, with OR as addition and AND as multiplication: the transitive closure $A^* = I \lor A \lor A^2 \lor \cdots$ in [[Bitset Auth]].

## Open
- [x] Determinants and inverses beyond $2 \times 2$ — cofactors and the adjugate are in [[Determinants and inverses for matrices]]; Gaussian elimination is still open there.
- [ ] Shear and reflection matrices alongside the rotation and dilation of [[Matrix Overview]].
- [ ] Diagonalisation and the characteristic polynomial, following on from [[eigen]].

## Related
- [[CAD Geometry]] — the main consumer of this area.
- [[Trigonometry]], [[Foundations]]
