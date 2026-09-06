---
tags:
  - concept
area: cad
status: stub
---
# B-Splines and NURBS

> [!summary] In a sentence
> A B-spline stitches many low-degree polynomial pieces into one curve with **local** control; a NURBS adds per-point weights so conic sections become exactly representable.

## Intuition
A single [[Bezier Curves|Bézier curve]] has one flaw for real modelling: moving *any* control
point changes the *whole* curve, and matching a complicated shape means raising the degree
until the polynomial is unwieldy. A B-spline instead chains together many low-degree segments,
so each control point only influences a local stretch. NURBS ("Non-Uniform Rational B-Spline")
then adds a weight per control point, which is what lets circles and other conics be exact
rather than approximated.

## Definition

$$

$$

## Key results / formulas

## Worked example

## Connections
- Generalises [[Bezier Curves]] — a Bézier curve is the special case with no interior knots and all weights equal.
- Part of [[CAD Geometry]]; the control points live in [[Euclidean space]] and the control polygon is a chain of [[Lines]].
- Segment joins are where continuity ($C^n$ vs $G^n$) matters — the tangent conditions in [[Bezier Curves]] are the degree-3 version of the same idea.
- Derivatives and tangents of the basis functions: [[Calculus]], [[chain-rule]].

## Open questions / TODO
- [ ] Write the Cox–de Boor recursion for the basis functions $N_{i,k}(t)$.
- [ ] Knot vectors: uniform vs non-uniform, and what multiplicity does to continuity.
- [ ] Show that a circle is exact as a NURBS but impossible as a polynomial Bézier.

## References
- 
