# Math Notes

An [Obsidian](https://obsidian.md) vault of study notes for the mathematics behind CAD:
linear algebra, multivariable & vector calculus, differential equations (ODEs/PDEs),
and CAD-specific geometry (parametric curves and surfaces, Bézier/B-splines/NURBS,
transformations).

## Opening the vault
1. Open Obsidian.
2. **Open folder as vault** → select this repository's root directory.
3. Trust the vault when prompted. Start at [[Home]].

## Layout
```
.
├── Home.md                 # dashboard / entry point
├── _templates/             # note templates (Concept Note, Worked Example)
├── _attachments/           # images, diagrams, screenshots
├── misc/                   # Foundations.md      — sets, exponent/log algebra
├── trig/                   # Trigonometry.md     — identities, compound angle
├── linear-algebra/         # Linear Algebra.md   — Euclidean space, lines
├── calculus/               # Calculus.md         — differentiation & integration
├── differential-equations/ # Differential Equations.md — first & second order ODEs
└── cad/                    # CAD Geometry.md     — Bézier, B-splines, NURBS
```
Each area folder has a Map-of-Content (MOC) note — the file named in the tree above — that
links to every topic in that folder. [[Home]] links to all six MOCs, so the whole vault is
reachable from one note.

## Linking conventions
So the graph view stays useful:
- Every note ends with a **`## Connections`** section of `[[wikilinks]]`.
- Every note links **up** to its area MOC, and its MOC links back **down** to it.
- Cross-area links are stated with the *reason* for the link, not just the name — e.g. "the
  integrating factor is the [[product rule]] read backwards" rather than a bare list entry.
- Unresolved links (faded nodes in the graph) are deliberate: they mark planned notes such as
  `Parametric Curves and Surfaces`, `Continuity and Smoothness`, `Vector Calculus` and
  `Partial Derivatives`.

## Conventions
- **Filenames:** Title Case so `[[wikilinks]]` read naturally.
- **Math:** MathJax — `$inline$` and `$$display$$`.
- **Status:** track mastery with `#status/stub`, `#status/learning`, `#status/solid`.

## Version control
The vault is a git repo. `.obsidian/workspace*.json` and the file-recovery cache are
git-ignored (they're per-machine UI state); the rest of `.obsidian/` is tracked so the
config travels with the vault.
