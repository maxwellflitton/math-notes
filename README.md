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
├── linear-algebra/         # matrices, eigenvalues, decompositions
├── calculus/               # multivariable & vector calculus
├── differential-equations/ # ODEs and PDEs
└── cad/                    # CAD geometry & math
```
Each area folder has a Map-of-Content note (e.g. `Linear Algebra.md`) that links to its topics.

## Conventions
- **Filenames:** Title Case so `[[wikilinks]]` read naturally.
- **Math:** MathJax — `$inline$` and `$$display$$`.
- **Status:** track mastery with `#status/stub`, `#status/learning`, `#status/solid`.

## Version control
The vault is a git repo. `.obsidian/workspace*.json` and the file-recovery cache are
git-ignored (they're per-machine UI state); the rest of `.obsidian/` is tracked so the
config travels with the vault.
