

Below is the pseudocode:

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