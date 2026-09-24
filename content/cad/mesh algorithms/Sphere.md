---
tags: [cad, mesh, geometry, rust, webgpu]
aliases: [uv sphere capacity, sphere sweep]
created: 2026-09-19
---

# UV Sphere Mesh — Capacity and Sweep

Unit sphere primitive for the CAD renderer. Built by sweeping a quarter circle from the equator to the north pole, rotating each node into a ring, then mirroring the rings south. Everything below is derived from a single input, $n_s$, so buffers can be allocated to exact size before any vertex is written.

> [!info] Conventions
> Z-up, poles at $(0,0,\pm R)$, rings lie in planes parallel to $xy$. Same convention as the [[Cylinder|cylinder]]. Buffers are `positions: Vec<Position>` and `indices: Vec<u32>` only — no edge array is needed for rendering.

## Definitions

| Symbol | Meaning | Value |
|---|---|---|
| $n_s$ | nodes per sweep (input, $n_s \ge 2$) | — |
| $n_r$ | nodes per ring | $4n_s$ |
| $\rho$ | number of rings | $2n_s - 3$ |
| $e$ | ring number of the equator | $n_s - 2$ |
| $R$ | sphere radius | — |

The sweep has $n_s$ nodes from equator to pole inclusive. Excluding the pole gives $n_s - 1$ rings per hemisphere; the equator is shared, so $\rho = 2(n_s - 1) - 1 = 2n_s - 3$.

![Quarter-circle sweep from equator to pole, rotated into rings of n_r vertices](_attachments/uv-sphere-sweep-geometry.svg)

## Sweep geometry

Two angles parameterise every vertex. Latitude $\varphi_k$ steps along the quarter arc; azimuth $\theta_i$ steps around a ring.

$$
\varphi_k = \frac{k}{n_s - 1}\cdot\frac{\pi}{2}, \qquad k = 0,\dots,n_s - 1
$$

$$
r_k = R\cos\varphi_k, \qquad z_k = R\sin\varphi_k, \qquad r_k^2 + z_k^2 = R^2
$$

$$
\theta_i = \frac{2\pi i}{n_r}, \qquad i = 0,\dots,n_r - 1
$$

$$
P_{k,i} = \big(\, r_k\cos\theta_i,\;\; r_k\sin\theta_i,\;\; \pm z_k \,\big)
$$

- $r_k$ is the ring radius, $z_k$ the ring height above the equator plane. Every vertex on a ring shares $z_k$.
- $k = 0$ is the equator ($r = R$, $z = 0$). $k = n_s - 1$ is the pole ($r = 0$, $z = R$), which collapses to one vertex and is written directly.
- The southern hemisphere reuses the same $r_k$ with $-z_k$.

> [!note] $R$ vs $r_k$
> $R$ is fixed; $r_k$ varies with latitude. In the Rust these are `ring_radius[k]` and `ring_height[k]`.

### Trig tables

The mesh is quadratic in $n_s$ but the trig is linear: two small tables cover it.

**Latitude table** — $n_s - 1$ entries, $k = 0 \dots n_s - 2$:

$$
r[k] = R\cos\varphi_k, \qquad z[k] = R\sin\varphi_k
$$

**Azimuth table** — $n_r$ entries, but only $n_s$ trig evaluations. Compute the first quadrant, then fill the other three by exact $90°$ rotation $(x, y) \to (-y, x)$:

$$
\begin{aligned}
c[p] &= \cos\theta_p, & s[p] &= \sin\theta_p \\
c[p + n_s] &= -s[p], & s[p + n_s] &= c[p] \\
c[p + 2n_s] &= -c[p], & s[p + 2n_s] &= -s[p] \\
c[p + 3n_s] &= s[p], & s[p + 3n_s] &= -c[p]
\end{aligned}
\qquad p = 0,\dots,n_s - 1
$$

Highest slot written is $(n_s - 1) + 3n_s = n_r - 1$: the table fills exactly. The rotated entries are swap-and-sign-flip copies, so the four quadrants are bit-exact and the axis points land on exactly $(1,0), (0,1), (-1,0), (0,-1)$. This is why rotation was chosen over mirroring (which double-writes axis points and reverses index order) and over incremental rotation (which drifts).

> [!warning] Precision
> Evaluate $\sin$ and $\cos$ in `f64` and cast the result to `f32`. Casting the angle first gives two rounding steps.

## Vertex capacity

Layout of the position buffer, north to south:

$$
V = \big[\; \text{N pole},\;\; (n_s - 2)\,n_r,\;\; \underbrace{n_r}_{\text{equator}},\;\; (n_s - 2)\,n_r,\;\; \text{S pole} \;\big]
$$

$$
\begin{aligned}
V_c &= 2 + 2(n_s - 2)\,n_r + n_r \\
    &= 2 + 8n_s(n_s - 2) + 4n_s \\
    &= 8n_s^2 - 12n_s + 2 \\
    &= 4n_s\,(2n_s - 3) + 2
\end{aligned}
$$

Structurally:

$$
V_c = \underbrace{n_r}_{\text{per ring}}\cdot\underbrace{\rho}_{\text{rings}} + \underbrace{2}_{\text{poles}}
$$

![Position buffer layout: north pole, northern rings, equator, southern rings, south pole](_attachments/uv-sphere-buffer-layout.svg)

Ring $m$ ($0 \le m < \rho$) starts at slot $1 + m\,n_r$. Its latitude is $k = |e - m|$, with $z$ negated for $m > e$. The equator starts at

$$
E = 1 + (n_s - 2)\,n_r
$$

and the northern and southern rings at latitude $k$ sit at $E - k\,n_r$ and $E + k\,n_r$: one base and a signed stride. Guard $k > 0$ so the equator is written once.

## Edge capacity

Edges are not stored, but the count is needed for the Euler check. Stacked bands share rims, so the equator is an ordinary ring with $n_r$ edges (its vertices have degree 6, not 8).

$$
\begin{aligned}
E_c &= \underbrace{2n_r}_{\text{pole spokes}} + \underbrace{\rho\,n_r}_{\text{ring edges}} + \underbrace{2n_r(\rho - 1)}_{\text{band rungs + diagonals}} \\
    &= 3\rho\,n_r \\
    &= 12n_s\,(2n_s - 3)
\end{aligned}
$$

## Face capacity

Each pole is a fan of $n_r$ triangles. Each of the $\rho - 1$ bands has $n_r$ quads, two triangles each.

$$
\begin{aligned}
F_c &= 2n_r + 2n_r(\rho - 1) \\
    &= 2\rho\,n_r \\
    &= 8n_s\,(2n_s - 3)
\end{aligned}
$$

## Index capacity

One triangle is three indices, $I_c = 3F_c$. Building it up directly:

$$
I_{\text{pole}} = 3n_r, \qquad I_{\text{band}} = 3\cdot 2n_r = 6n_r
$$

$$
\begin{aligned}
I_c &= 2(3n_r) + 6n_r(\rho - 1) \\
    &= 6n_r + 6n_r\rho - 6n_r \\
    &= 6n_r\,\rho \\
    &= 6(4n_s)(2n_s - 3) = 24n_s(2n_s - 3) = 48n_s^2 - 72n_s
\end{aligned}
$$

Byte size for `u32` indices: $B = 4I_c$.

> [!tip] $I_c$ from $V_c$
> For a closed sphere-like triangle mesh Euler gives $F = 2V - 4$, hence $I_c = 6V_c - 12$. This holds here but not for the open cylinder (where $F = V$), so derive capacities from $\rho$ and $n_r$ and keep $I_c = 6V_c - 12$ as a `debug_assert!`.

## Summary

$$
\boxed{
\begin{aligned}
V_c &= \rho\,n_r + 2 &&= 4n_s(2n_s - 3) + 2 \\
E_c &= 3\rho\,n_r &&= 12n_s(2n_s - 3) \\
F_c &= 2\rho\,n_r &&= 8n_s(2n_s - 3) \\
I_c &= 6\rho\,n_r &&= 24n_s(2n_s - 3)
\end{aligned}}
$$

$$
V_c - E_c + F_c = \rho n_r + 2 - 3\rho n_r + 2\rho n_r = 2 \quad\checkmark
$$

| $n_s$ | $n_r$ | $\rho$ | $V_c$ | $E_c$ | $F_c$ | $I_c$ | bytes (u32) |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 2 | 8 | 1 | 10 | 24 | 16 | 48 | 192 |
| 6 | 24 | 9 | 218 | 648 | 432 | 1296 | 5184 |
| 7 | 28 | 11 | 310 | 924 | 616 | 1848 | 7392 |

- $n_s = 2$ is the base case: a single ring with two fans, an octagonal bipyramid. Every formula checks against it.
- $V_c$ stays inside `u16` up to $n_s = 91$; `u32` indices are used regardless.
- Trig calls per sphere: $(n_s - 1) + n_s$, versus $\rho\,n_r \approx 8n_s^2$ vertices.

## Generation outline

Already implemented; recorded here for the shape of it.

1. Compute $n_r, \rho, e, V_c, I_c$; allocate both buffers to exact size.
2. Write the two poles at slots $0$ and $V_c - 1$.
3. Build the latitude table ($n_s - 1$ entries) and the azimuth table ($n_r$ entries, $n_s$ trig calls).
4. Single loop over $v = 1 \dots V_c - 2$, carrying a column counter $i$ and ring counter $m$:
   - $k = |e - m|$, sign of $z$ from $m > e$; position is $(r_k c_i,\; r_k s_i,\; \pm z_k)$.
   - $u$ = east neighbour ($v + 1$, wrapping at $i = n_r - 1$).
   - $m = 0$: north fan triangle. $m = \rho - 1$: south fan triangle ($u$ before $v$). Otherwise the quad $(v,\, v + n_r,\, u + n_r)$, $(v,\, u + n_r,\, u)$.
   - Index pointer only advances; `assert w == I_c` at the end.
5. No trig, division or modulo in the loop. Rings are placed north → south in slot order, so one winding covers all bands; only the two fans differ.

## Pseudocode

```pseudocode
precondition: n_s ≥ 2, R > 0

n_r ← 4·n_s
ρ   ← 2·n_s − 3            // number of rings
e   ← n_s − 2              // the equator's ring number
V_c ← ρ·n_r + 2
I_c ← 6·ρ·n_r

P ← array of size V_c
I ← array of size I_c

north ← 0
south ← V_c − 1
P[north] ← (0, 0,  R)
P[south] ← (0, 0, −R)

// latitude table
for k ← 0 to n_s − 2 do:
    φ    ← (π/2)·k / (n_s − 1)
    r[k] ← R cos φ
    z[k] ← R sin φ

// azimuth table: one quadrant of trig, three exact rotations
for p ← 0 to n_s − 1 do:
    θ ← (2π / n_r)·p
    c[p]         ←  cos θ ;   s[p]         ←  sin θ
    c[p + n_s]   ← −s[p]  ;   s[p + n_s]   ←  c[p]
    c[p + 2·n_s] ← −c[p]  ;   s[p + 2·n_s] ← −s[p]
    c[p + 3·n_s] ←  s[p]  ;   s[p + 3·n_s] ← −c[p]

// main loop: v walks the positions, w walks the indices
i ← 0                       // column within the ring
m ← 0                       // ring number, north to south
w ← 3·n_r                   // bands start after the north fan

for v ← 1 to V_c − 2 do:
    k ← |e − m|
    P[v] ← (r[k]·c[i],  r[k]·s[i],  z[k] if m ≤ e else −z[k])

    u ← v + 1                          // nearest neighbour
    if i = n_r − 1:  u ← v + 1 − n_r   // close the ring

    if m = 0:
        I[3i .. 3i+2] ← (north, v, u)

    if m = ρ − 1:
        I[w .. w+2] ← (south, u, v)
        w ← w + 3
    else:
        I[w .. w+5] ← (v, v + n_r, u + n_r,   v, u + n_r, u)
        w ← w + 6

    i ← i + 1
    if i = n_r:  i ← 0 ;  m ← m + 1

assert w = I_c
```

## Open questions

- With $n_r = 4n_s$ the quads near the equator are slightly rectangular ($\varphi$ steps $18°$ vs $\theta$ steps $15°$ at $n_s = 6$). $n_r = 4(n_s - 1)$ would square them. Not decided.
- Trig table caching across spheres, keyed on $n_s$, if it ever shows in a profile. Not a compile-time table — that would freeze the tessellation resolution.
- [[Mesh Writer Cursors]] — typed cursor idea for buffer writes. Not written yet.

## Connections
- Part of [[CAD Geometry]], and the sibling of [[Cylinder]]: same Z-up convention, same ring-of-$n_r$ vertex layout, same two-triangles-per-quad bands. The difference is closure — $\chi = 2$ here against $0$ for the open tube, which is exactly why $I_c = 6V_c - 12$ holds for the sphere and not the cylinder.
- The sweep is the unit circle used twice — [[Trigonometry]]. $r_k = R\cos\varphi_k$, $z_k = R\sin\varphi_k$ is a point on the quarter arc, so $r_k^2 + z_k^2 = R^2$ is Pythagoras in [[Euclidean space]]; $\theta_i$ around the ring is the [[Cylinder]] parametrisation again.
- The quadrant fill $(x, y) \to (-y, x)$ is $\cos(\theta + \tfrac{\pi}{2}) = -\sin\theta$ and $\sin(\theta + \tfrac{\pi}{2}) = \cos\theta$ — the [[compound angle formula|compound angle formulae]] at $B = 90°$. Because the second angle's sine and cosine are exactly $1$ and $0$, the copies are swap-and-sign only, which is what makes them bit-exact.
- For $R = 1$ every $P_{k,i}$ is its own outward unit normal — [[Unit vectors]] — and the positions are the component-form vectors of [[Basic Operations]].
- $V_c$, $E_c$, $F_c$, $I_c$ are closed-form counts fixed *before* the loop runs, so buffers are allocated once and `assert w = I_c` proves the fill was exact — the same move as the closed form in [[basic recursion for sum loop]] and in [[Cylinder]].
- Precondition, tables, then one loop with no trig, division or modulo is the picture → pseudocode → code order from [[introduction to algorithms]].
- Once the mesh is in a buffer it needs a name the GPU can write into a pixel: one `u32` holding the kind and the slot index — [[Bit Encoding]].
- Part of [[Home]].
