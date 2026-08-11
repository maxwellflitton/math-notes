---
tags:
  - moc
  - home
---
# Home

Entry point for the vault. Each area below has its own Map of Content (MOC) that links out
to the notes in that folder.

## Areas
- [[Foundations]] — sets, notation, exponent/log algebra. The prerequisites everything else leans on.
- [[Trigonometry]] — identities and the compound angle formulae.
- [[Linear Algebra]] — Euclidean space, lines, vectors.
- [[Calculus]] — differentiation and integration.
- [[Differential Equations]] — first and second order ODEs.
- [[CAD Geometry]] — the curves and surfaces this vault is ultimately aimed at.

## How it fits together
The dependency runs roughly left to right:

```
Foundations ──▶ Trigonometry ──▶ Calculus ──▶ Differential Equations
     │                              │                  │
     └──────▶ Linear Algebra ───────┴──────────────────┴──▶ CAD Geometry
```

- [[Foundations]] gives the notation ([[sets]]) and the exponent/log algebra ([[exp-algebria-rules]]) used everywhere.
- [[Calculus]] builds on limits ([[lim-zero-example]]) and gives the rules that [[Differential Equations]] runs backwards.
- [[Differential Equations]] models the physical systems ([[dampened harmonic oscillator]]) whose oscillations are re-expressed with [[compound angle formula|compound angle formulae]].
- [[CAD Geometry]] combines [[Linear Algebra]] (points, control polygons) with [[Calculus]] (tangents, continuity).

## Entry points by intent
| I want to… | Start at |
|---|---|
| Read dense maths notation | [[sets]] |
| Differentiate something | [[chain-rule]], [[product rule]] |
| Integrate something | [[Calculus]] |
| Solve an ODE | [[First order differential equations]], [[Second Order differential Equations]] |
| Understand a spring/oscillator | [[dampened harmonic oscillator]] |
| Draw a smooth curve | [[Bezier Curves]] |

## Conventions
See [[README]] for filename, math and status-tag conventions. Templates live in
`_templates/` ([[Concept Note]], [[Worked Example]]).
