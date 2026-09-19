---
title: Cylinder Mesh Algorithm
tags: [computer-graphics, meshing, algorithms, pseudocode]
created: 2026-09-13
---

# Cylinder Mesh Algorithm

---

## 1. The algorithm

![[cylinder-mesh-diagram.svg]]

```
Precondition: n ≥ 3

V ← array of size 2n
E ← array of size 4n
F ← array of size 2n

for p ← 0 to n-1 do

    θ ← (2π/n)·p

    q ← (p+1) mod n          // mod means set to 0 when p+1 = n

    V[p]   ← (R·cos θ, R·sin θ, offset)
    V[p+n] ← (R·cos θ, R·sin θ, offset + |a⃗|)

    s ← 4p                   // this could be pulled into a function
    E[s]   ← (p,   q)
    E[s+1] ← (p+n, q+n)
    E[s+2] ← (p,   p+n)
    E[s+3] ← (q,   p+n)

    t ← 2p
    F[t]   ← (p, q,   p+n)
    F[t+1] ← (q, q+n, p+n)
```

---

## 2. $E$ can be written as a union

$$E \;=\; \bigcup_{p=0}^{n-1} \Big\{\ \{p,\,q\},\ \ \{p{+}n,\ q{+}n\},\ \ \{p,\,p{+}n\},\ \ \{q,\,p{+}n\}\ \Big\}$$

$$q = (p+1) \bmod n$$

The fancy $\bigcup$ is union over an index, so for $n = 3$ we have the following:

| | $\{p,q\}$ | $\{p{+}n,\,q{+}n\}$ | $\{p,\,p{+}n\}$ | $\{q,\,p{+}n\}$ |
|---|---|---|---|---|
| $p=0,\ q=1$ | $\{0,1\}$ | $\{3,4\}$ | $\{0,3\}$ | $\{1,3\}$ |
| $p=1,\ q=2$ | $\{1,2\}$ | $\{4,5\}$ | $\{1,4\}$ | $\{2,4\}$ |
| $p=2,\ q=0$ | $\{2,0\}$ | $\{5,3\}$ | $\{2,5\}$ | $\{0,5\}$ |

All sides of the triangles can be derived from our $F$ array with the following:

$$\partial F \;=\; \bigcup_{(a,b,c)\,\in\,F} \Big\{\ \{a,b\},\ \ \{b,c\},\ \ \{a,c\}\ \Big\}$$

So for instance if a triangle has 3 points, the edges are:

$$F = \{\alpha,\ \beta\}$$

$$\alpha = (0,1,2) \;\to\; \{0,1\},\ \{1,2\},\ \{0,2\}$$

$$\beta = (1,2,3) \;\to\; \{1,2\},\ \{2,3\},\ \{1,3\}$$

so

$$\partial F = \Big\{\ \{0,1\},\ \{1,2\},\ \{0,2\},\ \{2,3\},\ \{1,3\}\ \Big\}$$

We can see that there are only 5 elements for $\partial F$ instead of 6. We can now claim that

$$E = \partial F \qquad\text{where}\qquad \partial F \subseteq E \quad\text{and}\quad E \subseteq \partial F$$

---

## 3. Proving the claim

$\partial F \subseteq E$ means that $\partial F$ is a subset of $E$, so we have **no junk**.

$E \subseteq \partial F$ means that there is **no gap**.

As $p$ and $q$ are indexes in the array, we get the following:

$$\{p,\,q\} \in \partial F \qquad\text{and}\qquad (p,\,q,\,p{+}n) \in F$$

This means we can reconstruct the edges of the triangles with the following function:

```
FUNCTION Edges(F):
    S ← empty set
    for each (a, b, c) in F do
        INSERT(S, {a, b})
        INSERT(S, {b, c})
        INSERT(S, {a, c})
    return S
```

Because $E = \partial F$, the `E` array can be dropped from the main loop entirely — the four `E[s+…]` writes are recovered by calling `Edges(F)` whenever they are needed.

---

## 4. Closed form — vertices

But looking at our algorithm initially, we never read anything past $p$, $q$ — so our algorithm is a **map**, not a fold. Therefore it is going to have a closed form.

We focus first on our $\theta$ with the following:

$$\theta = \frac{2\pi}{n}\,p = \frac{2\pi}{n}\,(k \bmod n)$$

with $k$ just being the index of the buffer. We can make the following substitution:

$$k \bmod n = k - n\left\lfloor \frac{k}{n} \right\rfloor$$

With this substitution we have the following:

$$\theta = \frac{2\pi}{n}\left(k - n\left\lfloor\frac{k}{n}\right\rfloor\right) = \frac{2\pi k}{n} - 2\pi\left\lfloor\frac{k}{n}\right\rfloor$$

For the cosine we have

$$\cos\left(\frac{2\pi k}{n} - 2\pi\left\lfloor\frac{k}{n}\right\rfloor\right)$$

and as $2\pi$ is a full rotation it collapses to

$$\cos\left(\frac{2\pi k}{n}\right)$$

So we can calculate a point anywhere in the buffer with the following:

$$V[k] = \left(R\cos\frac{2\pi k}{n},\ \ R\sin\frac{2\pi k}{n},\ \ \text{offset} + \left\lfloor\frac{k}{n}\right\rfloor|\vec{a}|\right)$$

So we can calculate the vertex with the following code:

```
FUNCTION Vertex(k, n, R, offset, h):
    θ ← (2π/n)·k
    r ← ⌊k/n⌋
    return (R·cos θ, R·sin θ, offset + r·h)
```

---

## 5. Closed form — faces

For the faces we have the general form:

$$\text{index} = \underbrace{\big((p + \Delta s) \bmod n\big)}_{\text{which position on the ring}} + \underbrace{n\,\Delta r}_{\text{which ring}}$$

The two triangles per quad are $(p,\ q,\ p{+}n)$ and $(q,\ q{+}n,\ p{+}n)$.

This can be boiled down to the following function:

```
Δs ← [ [0, 1, 0],
       [1, 1, 0] ]

Δr ← [ [0, 0, 1],
       [0, 1, 1] ]

FUNCTION FaceCorner(m, c, n):
    p ← ⌊m/2⌋
    j ← m mod 2
    return ((p + Δs[j][c]) mod n) + n·Δr[j][c]
```

Then you can build the entire cylinder with the following:

```
V ← array of size 2n
F ← array of size 2n

for k ← 0 to 2n-1 do
    V[k] ← Vertex(k, n, R, offset, h)

for m ← 0 to 2n-1 do
    F[m] ← (FaceCorner(m,0,n), FaceCorner(m,1,n), FaceCorner(m,2,n))

return (V, F)
```

Both loops can run in different threads or on a GPU.


---

## Connections
- Part of [[CAD Geometry]] — a mesh is what a surface becomes once it has to be drawn, and choosing $n$ is the same accuracy-versus-cost trade-off as evaluating a [[Bezier Curves|Bézier curve]] at finitely many parameters.
- The ring $R(\cos\theta, \sin\theta, 0)$ is the unit circle scaled to radius $R$ — the parametrisation of [[Trigonometry]] — and §4 turns entirely on $\cos$ having period $2\pi$, so the $2\pi\lfloor k/n \rfloor$ term can be dropped.
- $V[k]$ is a position vector in component form: [[Basic Operations]], living in [[Euclidean space]]. The $z = \text{offset} + \lfloor k/n \rfloor|\vec{a}|$ split is a base position plus a displacement along the axis — [[Displacement vs Position Vectors]].
- §2 and §3 are set notation doing real work: $\{p, q\}$ is an *unordered* pair, so the union dedupes on its own and $\{2,0\}$ and $\{0,2\}$ are one edge. The notation is unpacked in [[sets]].
- Proving $E = \partial F$ by two inclusions — no junk, no gaps — is the set-theoretic sibling of the induction in [[basic recursion for sum loop]]; both replace a loop with something you can evaluate directly once you have proved the loop was doing nothing else.
- Deriving $V[k]$ and `FaceCorner` is exactly that closed-form move: $O(1)$ per element instead of walking the loop, which is also what makes both loops independent enough to run on a GPU.
- Picture, then pseudocode, then code is the order of description set out in [[introduction to algorithms]] — this note follows it top to bottom.
- The closed version of the same construction: [[Sphere]] stacks these rings along a latitude sweep and caps them with pole fans, which is what makes its Euler characteristic $2$ rather than $0$.
- Part of [[Home]].

