# Math Notes

An [Obsidian](https://obsidian.md) vault of study notes for the mathematics behind CAD:
linear algebra, multivariable & vector calculus, differential equations (ODEs/PDEs),
and CAD-specific geometry (parametric curves and surfaces, Bézier/B-splines/NURBS,
transformations).

The vault is published as a website with [Quartz 5](https://quartz.jzhao.xyz):
**https://maxwellflitton.github.io/math-notes/**

## Editing

Open `content/` as an Obsidian vault (**Open folder as vault** → select `content/`).
Trust the vault when prompted. Start at `index.md`.

The vault is the source of truth — write notes exactly as you always have. Wikilinks,
aliased wikilinks, image embeds, `$…$` / `$$…$$` LaTeX, callouts and inline SVG all
carry through to the site unchanged.

## Local preview

```bash
npm install
npx quartz build --serve
```

Then open http://localhost:8080. The dev server watches `content/` and reloads on save.

## Publishing

Commit changes and push to `main`. GitHub Actions
([.github/workflows/deploy.yml](.github/workflows/deploy.yml)) rebuilds the site and
deploys it to GitHub Pages automatically. Nothing generated is committed — `public/` is
built in CI as a Pages artifact.

One-time repo setup: **Settings → Pages → Source → GitHub Actions**.

## Layout

```
.
├── content/                    # ← the Obsidian vault; open THIS folder in Obsidian
│   ├── index.md                # homepage / dashboard (aliased as "Home")
│   ├── _templates/             # note templates (excluded from the site)
│   ├── _attachments/           # images, diagrams, screenshots
│   ├── misc/                   # Foundations.md      — sets, exponent/log algebra
│   ├── trig/                   # Trigonometry.md     — identities, compound angle
│   ├── linear-algebra/         # Linear Algebra.md   — Euclidean space, vectors, lines
│   ├── calculus/               # Calculus.md         — differentiation & integration
│   ├── differential-equations/ # Differential Equations.md — first & second order ODEs
│   ├── cad/                    # CAD Geometry.md     — Bézier, B-splines, NURBS
│   └── Algorithms/             # recursion, complexity
│
├── quartz/                     # vendored Quartz 5 source — don't edit by hand
├── quartz.config.yaml          # ← all site configuration lives here
├── quartz.config.default.yaml  # upstream reference copy
└── .github/workflows/deploy.yml
```

Each area folder has a Map-of-Content (MOC) note — the file named in the tree above — that
links to every topic in that folder. `index.md` links to all the MOCs, so the whole vault is
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
- **Math:** KaTeX — `$inline$` and `$$display$$`.
- **Status:** track mastery with `#status/stub`, `#status/learning`, `#status/solid`.
- **Diagrams:** prefer inline `<svg>` with `stroke="currentColor"` / `fill="currentColor"`
  so the diagram adapts to light and dark mode. See
  `content/linear-algebra/vectors/Displacement vs Position Vectors.md` for the pattern.
- **Image embeds:** `![[image.png]]` works for raster images. For **`.svg` files** use a
  markdown link instead — `![alt](_attachments/diagram.svg)` — see
  [Known limitations](#known-limitations).

## Configuration

Everything is configured in **[`quartz.config.yaml`](quartz.config.yaml)**. There are two
sections: `configuration:` (site-wide settings) and `plugins:` (a flat list, where each
entry is a feature).

### Base URL / custom domain

```yaml
configuration:
  baseUrl: maxwellflitton.github.io/math-notes
```

That is the **only** place the site URL is set. To move to a custom domain such as
`notes.example.com`:

1. Change `baseUrl` to `notes.example.com` (no protocol, no trailing slash).
2. Re-enable the CNAME emitter — set `enabled: true` on the `@quartz-community/cname`
   plugin. It writes `public/CNAME` from `baseUrl`, which is what tells GitHub Pages the
   custom domain. It is **disabled** while hosting on a `github.io` subpath, because a
   `CNAME` of `maxwellflitton.github.io` would break the deployment.
3. Point DNS at GitHub and set the domain under Settings → Pages → Custom domain.

### Sidebar components

Each UI feature is a plugin entry with a `layout` block. `position` picks the region
(`left`, `right`, `beforeBody`, `afterBody`, `header`, `footer`) and `priority` orders
components within it (lower = higher up).

| Feature | Plugin | Where |
| --- | --- | --- |
| Search | `@quartz-community/search` | left, in the `toolbar` group |
| Explorer (file tree) | `@quartz-community/explorer` | left, priority 50 |
| Dark/light toggle | `@quartz-community/darkmode` | left, `toolbar` group |
| Graph (local + global) | `@quartz-community/graph` | right, priority 10 |
| Table of contents | `@quartz-community/table-of-contents` | right, priority 30 |
| Backlinks | `@quartz-community/backlinks` | right, priority 50 |
| Breadcrumbs | `@quartz-community/breadcrumbs` | beforeBody, priority 5 |

**To remove a sidebar component**, set `enabled: false` on its plugin entry.
**To move one**, change its `layout.position` / `layout.priority`.
**To add a new one**, `npx quartz plugin add github:quartz-community/<name>` then add an
entry with a `layout` block.

Other notable settings:

- **LaTeX** — `@quartz-community/latex`, `renderEngine: katex`. Swap to `mathjax` or add
  `customMacros` there.
- **Obsidian syntax** — `@quartz-community/obsidian-flavored-markdown` (wikilinks, callouts,
  embeds, tags) plus `@quartz-community/crawl-links` with
  `markdownLinkResolution: shortest`, which mirrors Obsidian's `"newLinkFormat": "shortest"`
  and is what makes basename-only `[[links]]` resolve. Don't change one without the other.
- **`hard-line-breaks` is deliberately disabled.** These notes wrap prose across lines and
  the vault sets `strictLineBreaks: false`; enabling it would turn every newline into a
  `<br>`.
- **Keeping notes private** — add a glob to `configuration.ignorePatterns` (that is how
  `_templates` is excluded), or set `draft: true` in a note's frontmatter. Non-Markdown
  files in `content/` are always published, so keep private assets out of the vault.

### Upgrading Quartz

```bash
npx quartz upgrade
```

This pulls from the `upstream` remote (`https://github.com/jackyzha0/quartz.git`).

## Known limitations

- **`![[diagram.svg]]` embeds do not resolve.** Quartz renders `.svg` wikilink embeds as
  `<object data="…">`, and its link rewriter only fixes `src` on `img`/`video`/`audio`/
  `iframe` — so the path stays relative to the note and 404s. Use a markdown image link
  instead: `![alt](_attachments/diagram.svg)`. This resolves correctly in both Obsidian and
  Quartz. Raster embeds (`![[image.png]]`) are unaffected.
- **`_attachments/unit-circle-sincos.svg` uses hard-coded colours** (`#999`, `#444`,
  `#1f77b4`, `#d62728`), so unlike the inline SVG diagrams it does not adapt to dark mode.
  Switching its strokes to `currentColor` would fix it.
- **Inline SVG `id`s are global.** `Displacement vs Position Vectors.md` defines markers
  `arrowN` / `arrowP`; if a second inline SVG on the same page reuses those ids they will
  collide. Give each diagram unique marker ids.

## Version control

`content/.obsidian/workspace*.json` and the file-recovery cache are git-ignored (per-machine
UI state); the rest of `content/.obsidian/` is tracked so the config travels with the vault.
`node_modules/`, `public/` and `.quartz/` are never committed.

Quartz itself is MIT licensed — see [LICENSE-quartz.txt](LICENSE-quartz.txt).
