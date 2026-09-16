---
tags:
  - maths
  - graphics
  - cad
topic: orthographic projection
---

# Orthographic projection derivation

> [!info] How to read this
> This follows the handwritten notes (pages 1–7), with errors corrected in place. Everything after the end of page 7 is marked **Added** and finishes the derivation.

---

## 1. Along and across (page 1)

Take the unit vector $\hat{e}$, so $\hat{e} \cdot \hat{e} = 1$. Any vector $\vec{p}$ can be written as a piece parallel to $\hat{e}$ plus a piece perpendicular to it:

$$
\vec{p} = \underbrace{(\vec{p} \cdot \vec{e})\,\vec{e}}_{\text{along } e} + \underbrace{\vec{p} - (\vec{p} \cdot \vec{e})\,\vec{e}}_{\text{across } e}
$$

Remember that $\vec{e} = \hat{e}$, so $|\hat{e}| = 1$, which gives

$$
\vec{p} \cdot \vec{e} = |\vec{p}|\,|\vec{e}| \cos\theta = |\vec{p}| \cos\theta
$$

This scalar is the length of $\vec{p}$'s shadow along $\vec{e}$, so $(\vec{p} \cdot \vec{e})\,\vec{e}$ is the projection of $\vec{p}$ in the direction of $\vec{e}$. This means we can have the following:

$$
\vec{p}_{\parallel} = (\vec{p} \cdot \vec{e})\,\vec{e}
$$

As

$$
\vec{p} - (\vec{p} \cdot \vec{e})\,\vec{e} = \vec{p} - \vec{p}_{\parallel}
$$

taking away the parallel part of the vector leaves only the orthogonal part. So we can say that:

$$
\vec{p}_{\perp} = \vec{p} - (\vec{p} \cdot \vec{e})\,\vec{e}
$$

We can prove this with the scalar (dot) product of $\vec{p}_{\perp}$ with $\hat{e}$, since two vectors are perpendicular exactly when their dot product is zero:

$$
\vec{p}_{\perp} \cdot \vec{e} = \vec{p} \cdot \vec{e} - (\vec{p} \cdot \vec{e})(\vec{e} \cdot \vec{e})
$$

As $\vec{e} \cdot \vec{e} = 1$:

$$
\vec{p}_{\perp} \cdot \vec{e} = \vec{p} \cdot \vec{e} - \vec{p} \cdot \vec{e} = 0
$$

### Example

$$
\vec{e} = \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}, \qquad \vec{p} = \begin{bmatrix} 3 \\ -2 \\ 7 \end{bmatrix} \implies \vec{p} \cdot \vec{e} = 7
$$

$$
\vec{p}_{\perp} = \begin{bmatrix} 3 \\ -2 \\ 7 \end{bmatrix} - (\vec{p} \cdot \vec{e}) \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix} = \begin{bmatrix} 3 \\ -2 \\ 7 \end{bmatrix} - 7 \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix} = \begin{bmatrix} 3 \\ -2 \\ 0 \end{bmatrix}
$$

Because we are looking directly down the $z$ axis with $\hat{e}$, the $x, y$ is directly projected at us, but the $z$ is completely hidden from the projection — but not lost. It is kept as depth.

---

## 2. Matrix form (pages 2–3)

What we can say is the following:

- $\vec{p}_{\perp}$ = where the point lands on the image plane.
- $\vec{p}_{\parallel}$ = the depth vector. To write depth to a texture we take its signed length, a scalar:

$$
d = \vec{p} \cdot \vec{e}
$$

The screen coordinates come from two unit vectors lying in the plane:

$$
x_{\text{screen}} = \vec{p} \cdot \hat{u}, \qquad y_{\text{screen}} = \vec{p} \cdot \hat{v}
$$

> [!note] Naming
> From section 3 onwards, $\hat{u}$ becomes $\vec{r}$ (camera right) and $\hat{v}$ becomes $\vec{u}'$ (camera up). Same idea, different letters.

Now we can translate this to matrix form:

$$
e = \begin{bmatrix} e_{1} \\ e_{2} \\ e_{3} \end{bmatrix}, \qquad p = \begin{bmatrix} p_{1} \\ p_{2} \\ p_{3} \end{bmatrix} \implies e^{T} p = \begin{bmatrix} e_{1} & e_{2} & e_{3} \end{bmatrix} \begin{bmatrix} p_{1} \\ p_{2} \\ p_{3} \end{bmatrix}
$$

$$
e^{T} p = e_{1} p_{1} + e_{2} p_{2} + e_{3} p_{3} = \vec{p} \cdot \vec{e}
$$

The matrix pipeline checks out: $(1 \times 3)(3 \times 1) = 1 \times 1$.

As $e^{T} p = \vec{e} \cdot \vec{p}$:

$$
(\vec{p} \cdot \vec{e})\,\vec{e} = \vec{e}\,(e^{T} p) = (e e^{T})\,p
$$

The second step only regroups the brackets, $A(BC) = (AB)C$, keeping the same three factors in the same order. What we can't do is swap the order of matrix multiplication, since $AB \neq BA$ in general. So:

$$
e e^{T} = (3 \times 1)(1 \times 3) = 3 \times 3, \qquad e^{T} e = (1 \times 3)(3 \times 1) = 1 \times 1
$$

$ee^{T}$ is the outer product (a matrix) and $e^{T}e$ is the inner product (a number).

So when

$$
e = \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix} \implies e^{T} = \begin{bmatrix} 0 & 0 & 1 \end{bmatrix} \implies e e^{T} = (3 \times 1)(1 \times 3) = (3 \times 3)
$$

$$
e e^{T} = \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix} \begin{bmatrix} 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}
$$

In general, entry $(i, j)$ of $ee^{T}$ is $e_{i} e_{j}$, so the matrix is symmetric.

### The projection matrix

Now we remind ourselves of some of what we worked out:

$$
e^{T} p = \vec{p} \cdot \vec{e} \implies p_{\parallel} = e e^{T} p \quad \text{and} \quad p_{\perp} = p - e e^{T} p
$$

We cannot subtract a matrix from a vector, so we replace $\vec{p}$ with $Ip$, where $I$ is the identity matrix:

$$
p_{\perp} = Ip - e e^{T} p
$$

Both terms end in $p$, so it factors out on the right:

$$
p_{\perp} = (I - e e^{T})\,p
$$

Therefore the projection matrix takes the form:

$$
\Pi_{e} = I - e e^{T}
$$

> [!note] Don't confuse the two
> $ee^{T}$ keeps the **along** piece (depth). $\Pi_{e} = I - ee^{T}$ keeps the **across** piece (image plane). They add to the identity: $ee^{T} + \Pi_{e} = I$.

We can calculate the example:

$$
\Pi_{e} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} - \begin{bmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{bmatrix}
$$

We can check a general case with the following:

$$
\begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{bmatrix} \begin{bmatrix} p_{1} \\ p_{2} \\ p_{3} \end{bmatrix} = \begin{bmatrix} p_{1} \\ p_{2} \\ 0 \end{bmatrix}
$$

> [!info] the two properties
> $$\Pi_{e}\,e = e - e(e^{T}e) = e - e = 0$$
> Sliding a point along the line of sight doesn't change where it lands, so $\Pi_{e}$ has no inverse — depth can't be recovered from the image.
> $$\Pi_{e}^{2} = I - 2ee^{T} + e(e^{T}e)e^{T} = I - ee^{T} = \Pi_{e}$$
> Projecting twice does nothing new. $P^{2} = P$ makes it a projection; being symmetric makes it an *orthogonal* projection.

> [!note] Where $\Pi_{e}$ fits
> $\Pi_{e}$ is scaffolding for understanding what orthographic projection *is*. The GPU never multiplies by it. In practice the view matrix (below) rotates into camera space and "drop $z$" does the same job.

---

## 3. The camera axes and the view rotation

**Getting the camera/view matrix: the $(x, y, z)$ of a point relative to the camera.**

Now we need to construct the image plane. It needs the following conditions:

$$
\vec{r} \cdot \vec{e} = 0, \qquad \vec{u}' \cdot \vec{e} = 0, \qquad \vec{r} \cdot \vec{u}' = 0, \qquad |\vec{r}| = |\vec{u}'| = 1
$$

This basically means that we have two unit vectors orthogonal to each other, with no component along $\vec{e}$ (both lie in the image plane). Together with $\vec{e}$, the three form an **orthonormal basis** — the camera's own set of axes, a datum that moves with the camera.

We go back to step one again, but with a rotated set of axes, and split in all three directions:

$$
\vec{p} = (\vec{p} \cdot \vec{r})\,\vec{r} + (\vec{p} \cdot \vec{u}')\,\vec{u}' + (\vec{p} \cdot \vec{e})\,\vec{e}
$$

It's the same idea as $\vec{p} = x\hat{i} + y\hat{j} + z\hat{k}$, but for the camera.

We can check this by dotting both sides with $\hat{r}$, where $\hat{r} \cdot \hat{r} = 1$:

$$
\vec{p} \cdot \hat{r} = (\vec{p} \cdot \vec{r})(1) + (\vec{p} \cdot \vec{u}')(0) + (\vec{p} \cdot \vec{e})(0) = \vec{p} \cdot \hat{r}
$$

and the same works for $\vec{u}'$ and $\vec{e}$.

Comparing with step one, the last term is $\vec{p}_{\parallel}$, so the first two must be the across piece:

$$
\Pi_{e}\,\vec{p} = (\vec{p} \cdot \vec{r})\,\vec{r} + (\vec{p} \cdot \vec{u}')\,\vec{u}'
$$

The coefficients in front of each axis are the coordinates, so we get the coordinates relative to the camera directly from the dot products:

$$
x = \vec{p} \cdot \vec{r}, \qquad y = \vec{p} \cdot \vec{u}', \qquad z = \vec{p} \cdot \vec{e}
$$

$x$ and $y$ are the screen position; $z$ is the depth.

We now have a coordinate system that accounts for **which way the camera faces** (its position comes later, in section 4). However, the four conditions don't pin down a unique pair: if the camera stays pointing the same way but rotates about $\vec{e}$ (roll), the picture changes, yet every condition still holds. We haven't taken that into account yet.

### Fixing the roll

We now have to construct the camera's right and up axes. It is general convention for GL that the vector $\vec{e}$ points **out of the screen at you**, so $\vec{f}$, the direction the camera faces (forward), takes the form:

$$
\vec{f} = -\vec{e}
$$

The engine takes an up **hint** $\vec{u}$ — anything roughly upward, not necessarily unit length (not to be confused with $\vec{u}'$). We can then find the unit vector $\vec{r}$ (the $x$ direction on the screen):

$$
\vec{r} = \frac{\vec{f} \times \vec{u}}{|\vec{f} \times \vec{u}|} \implies \vec{u}' = \vec{r} \times \vec{f}
$$

> [!info] Added — why the construction works
> - A cross product is perpendicular to both inputs. So $\vec{r} \perp \vec{f}$ (it lies in the image plane) and $\vec{r} \perp \vec{u}$ (it points sideways).
> - $\vec{r}$ needs normalising because $|\vec{f} \times \vec{u}| = |\vec{f}|\,|\vec{u}| \sin\theta$, and the hint may not be unit length or perpendicular to $\vec{f}$.
> - $\vec{u}'$ needs no normalising: $\vec{r}$ and $\vec{f}$ are perpendicular unit vectors, so $|\vec{r} \times \vec{f}| = (1)(1)\sin 90^{\circ} = 1$.
> - Only the across part of the hint matters. The along part dies because $\vec{f} \times \vec{f} = \vec{0}$. E.g. $\vec{u} = (0, 1, 5)$ gives the same $\vec{r}$ as $\vec{u} = (0, 1, 0)$ when $\vec{f} = (0, 0, -1)$.
> - **Forbidden hint:** a hint parallel to $\vec{e}$ gives $\vec{f} \times \vec{u} = \vec{0}$ and a divide by zero. In a Z-up CAD world, looking straight down with hint $(0,0,1)$ breaks. Fix: swap to a different hint, e.g. $(0,1,0)$, when the view is nearly vertical.

> [!info] check by hand
> $\vec{f} = (0, 0, -1)$, $\vec{u} = (0, 1, 0)$:
> $$\vec{f} \times \vec{u} = \begin{bmatrix} (0)(0) - (-1)(1) \\ (-1)(0) - (0)(0) \\ (0)(1) - (0)(0) \end{bmatrix} = \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix} = \vec{r}$$
> $$\vec{u}' = \vec{r} \times \vec{f} = \begin{bmatrix} (0)(-1) - (0)(0) \\ (0)(0) - (1)(-1) \\ (1)(0) - (0)(0) \end{bmatrix} = \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}$$
> The screen axes are the world $x$ and $y$ axes, as a plan view should give.

This leads to the following **view rotation matrix** of the camera:

$$
\begin{bmatrix} x \\ y \\ z \end{bmatrix} = \begin{bmatrix} r_{1} & r_{2} & r_{3} \\ u'_{1} & u'_{2} & u'_{3} \\ e_{1} & e_{2} & e_{3} \end{bmatrix} \begin{bmatrix} p_{1} \\ p_{2} \\ p_{3} \end{bmatrix}
$$

where each row is one camera axis written in world coordinates:

$$
\text{camera right} = \vec{r} = \begin{bmatrix} r_{1} \\ r_{2} \\ r_{3} \end{bmatrix}, \qquad \text{camera up} = \vec{u}' = \begin{bmatrix} u'_{1} \\ u'_{2} \\ u'_{3} \end{bmatrix}, \qquad \text{view direction} = \vec{e} = \begin{bmatrix} e_{1} \\ e_{2} \\ e_{3} \end{bmatrix}
$$

> [!note] Remember
> - $\vec{e}$ points **backwards** (out of the screen). Forward is $\vec{f} = -\vec{e}$.
> - This matrix is a **rotation**, not a projection. It keeps all three coordinates — nothing is thrown away yet. World point in, camera-space point out.

---

## 4. Measure from the eye: the full view matrix

Finally we apply the offset of the camera/eye to the view matrix. $\vec{E}$ is the eye position (a point) and $R$ is the scalar orbit radius.

> [!note] Watch the notation
> - $\vec{E}$ (capital) = eye **position**. $\vec{e}$ (lower case) = unit view **direction**.
> - $R$ = orbit radius, a scalar (distance from target to eye).
> - $\vec{t}$ = target, the point the camera orbits and looks at.

First we measure the point from the eye:

$$
x_{c} = (\vec{p} - \vec{E}) \cdot \vec{r}, \qquad y_{c} = (\vec{p} - \vec{E}) \cdot \vec{u}', \qquad z_{c} = (\vec{p} - \vec{E}) \cdot \vec{e}
$$

The subscript $c$ means camera space.

Subtracting $\vec{E}$ is a translation, and a $3 \times 3$ matrix can't do that (it always sends $\vec{0}$ to $\vec{0}$). So we tack a 1 onto the end of every point, giving $(p_{1}, p_{2}, p_{3}, 1)$ — homogeneous coordinates. With the point as a column on the right (the WGSL convention):

$$
\begin{bmatrix} x_{c} \\ y_{c} \\ z_{c} \\ 1 \end{bmatrix} = \begin{bmatrix} r_{x} & r_{y} & r_{z} & -\vec{r} \cdot \vec{E} \\ u'_{x} & u'_{y} & u'_{z} & -\vec{u}' \cdot \vec{E} \\ e_{x} & e_{y} & e_{z} & -\vec{e} \cdot \vec{E} \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} p_{x} \\ p_{y} \\ p_{z} \\ 1 \end{bmatrix}
$$

The top-left $3 \times 3$ block is the view rotation from section 3. The last column is the shift to the eye.

> [!note] Rows vs columns
> The text writes this as $(\vec{p}\;1)\,V$, with the point as a **row on the left**. In that form the axes are the **columns** of $V$ and the translation is the **bottom row**. The version above is its transpose, for a **column on the right**. Both are right; don't mix them.

We can check the first row with the following:

$$
x_{c} = r_{x} p_{x} + r_{y} p_{y} + r_{z} p_{z} - \vec{r} \cdot \vec{E}
$$

As $r_{x} p_{x} + r_{y} p_{y} + r_{z} p_{z} = \vec{p} \cdot \vec{r}$:

$$
x_{c} = \vec{p} \cdot \vec{r} - \vec{E} \cdot \vec{r} = (\vec{p} - \vec{E}) \cdot \vec{r}
$$

The translation entry is $-\vec{r} \cdot \vec{E}$, not $-E_{x}$, because the shift is measured along the camera's axes rather than the world's.

Before, we only accounted for where the camera faced; now we account for where the camera is positioned.

### The eye drops out of the screen position

The unique part about orthographic is that the rays are **parallel**. There is no fanning out, unlike perspective projection. So the radius (distance from eye to object) does not change the screen position — moving closer does not zoom. We can see this with the following.

The engine places the eye on the far side of the target, along the viewing direction:

$$
\vec{E} = \vec{t} + R\,\vec{e}
$$

Substitute into $x_{c}$ and expand:

$$
x_{c} = (\vec{p} - \vec{t} - R\,\vec{e}) \cdot \vec{r} = (\vec{p} - \vec{t}) \cdot \vec{r} - R\,(\vec{e} \cdot \vec{r})
$$

As $\vec{e} \cdot \vec{r} = 0$:

$$
x_{c} = (\vec{p} - \vec{t}) \cdot \vec{r}
$$

and we can see that $R$ has vanished.

---

> [!info] Added from here on
> Everything below finishes the derivation from where page 7 stops.

The same happens for $y_{c}$, since $\vec{e} \cdot \vec{u}' = 0$:

$$
y_{c} = (\vec{p} - \vec{t}) \cdot \vec{u}' - R\,(\vec{e} \cdot \vec{u}') = (\vec{p} - \vec{t}) \cdot \vec{u}'
$$

For the orthographic camera, then, the screen position depends only on the target and the axes, never on $R$.

**What this means for the CAD app:**

- The engine can clamp $R$ to a minimum and push the camera back out of the geometry at no visible cost.
- **Zoom cannot come from moving the camera.** It has to come from the projection bounds (section 6).
- This is unique to orthographic. In perspective, moving closer does make things bigger, because the rays fan out from the eye.

---

## 5. Depth keeps the eye, and picks up a sign

The third coordinate does **not** cancel, because $\vec{e} \cdot \vec{e} = 1$, not 0:

$$
z_{c} = (\vec{p} - \vec{t}) \cdot \vec{e} - R\,(\vec{e} \cdot \vec{e}) = (\vec{p} - \vec{t}) \cdot \vec{e} - R
$$

So $R$ survives in depth. That makes sense: pulling the camera back moves nothing on screen, but everything gets further away.

### The sign

$\vec{e}$ points from the target **towards** the camera. So a point in front of the camera has $z_{c} < 0$. The target itself ($\vec{p} = \vec{t}$) sits at

$$
z_{c} = 0 - R = -R
$$

To get a positive distance in front of the eye, flip the sign:

$$
d = -z_{c} = -(\vec{p} - \vec{E}) \cdot \vec{e} = (\vec{E} - \vec{p}) \cdot \vec{e}
$$

$d$ is positive and grows away from the viewer. This is the refined version of the depth from section 2: before it was $\vec{p} \cdot \vec{e}$, measured from the origin; now it's measured from the eye, with the sign flipped. It's the number the depth buffer stores once it's scaled into range (section 6).

### Worked example

Target at the origin, looking straight down $z$, radius 10:

$$
\vec{t} = (0, 0, 0), \quad \vec{e} = (0, 0, 1), \quad R = 10, \quad \vec{E} = \vec{t} + R\vec{e} = (0, 0, 10), \quad \vec{p} = (3, -2, 7)
$$

With hint $\vec{u} = (0, 1, 0)$, section 3 gives $\vec{r} = (1, 0, 0)$ and $\vec{u}' = (0, 1, 0)$. The translation column is

$$
-\vec{r} \cdot \vec{E} = 0, \qquad -\vec{u}' \cdot \vec{E} = 0, \qquad -\vec{e} \cdot \vec{E} = -10
$$

$$
\begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & -10 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 3 \\ -2 \\ 7 \\ 1 \end{bmatrix} = \begin{bmatrix} 3 \\ -2 \\ -3 \\ 1 \end{bmatrix}
$$

So $x_{c} = 3$, $y_{c} = -2$, $z_{c} = -3$ and $d = 3$: the eye is at height 10, the point at 7, so it's 3 units in front.

Now pull the camera back to $R = 50$, so $\vec{E} = (0, 0, 50)$:

$$
x_{c} = 3, \qquad y_{c} = -2, \qquad z_{c} = 7 - 50 = -43, \qquad d = 43
$$

The screen position is unchanged; only the depth moved.

---

## 6. The orthographic projection matrix

The view matrix gives camera space but throws nothing away. The **projection** matrix scales camera space into a fixed box:

- $x_{c}$ and $y_{c}$ are divided by the half-width $w$ and half-height $h$ of the visible region, so it lands in $[-1, 1]$.
- Depth $d = -z_{c}$ is mapped from $[n, f]$ (near and far planes, as distances in front of the eye) to $[0, 1]$, WebGPU's convention.

> [!note] Check this against the text
> This section is written ahead of the text's own step 5. The idea will be the same; the text may use off-centre bounds $(l, r, b, t)$ instead of a centred $w, h$.

For a view centred on the camera:

$$
P = \begin{bmatrix} \dfrac{1}{w} & 0 & 0 & 0 \\ 0 & \dfrac{1}{h} & 0 & 0 \\ 0 & 0 & \dfrac{-1}{f - n} & \dfrac{-n}{f - n} \\ 0 & 0 & 0 & 1 \end{bmatrix}
$$

The depth row gives

$$
z_{\text{ndc}} = \frac{-z_{c} - n}{f - n} = \frac{d - n}{f - n}
$$

Check it on the two planes. Near plane, $z_{c} = -n$:

$$
\frac{n}{f - n} - \frac{n}{f - n} = 0
$$

Far plane, $z_{c} = -f$:

$$
\frac{f}{f - n} - \frac{n}{f - n} = 1
$$

**This is where zoom lives.** Shrinking $w$ and $h$ shows less of the world, which magnifies it. Keep the ratio $w / h$ equal to the window's aspect ratio or the image stretches.

### Worked example (continued)

Take $w = h = 5$, $n = 1$, $f = 21$, and the camera-space point $(3, -2, -3)$ from above:

$$
\begin{bmatrix} \tfrac{1}{5} & 0 & 0 & 0 \\ 0 & \tfrac{1}{5} & 0 & 0 \\ 0 & 0 & -\tfrac{1}{20} & -\tfrac{1}{20} \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 3 \\ -2 \\ -3 \\ 1 \end{bmatrix} = \begin{bmatrix} 0.6 \\ -0.4 \\ 0.1 \\ 1 \end{bmatrix}
$$

Depth check: $\dfrac{d - n}{f - n} = \dfrac{3 - 1}{20} = 0.1$. ✓

The fourth component stays 1, so unlike perspective there is no divide by $w$.

---

## 7. The full pipeline

$$
\vec{p}_{\text{clip}} = P\,V\,\vec{p}
$$

Read right to left: $V$ moves the point into camera space, then $P$ scales it into the clip box. The order matters, because $PV \neq VP$.

| Matrix | What it does | Changes when |
| --- | --- | --- |
| $V$ (view) | Rotates and shifts world points into camera space | The user orbits or pans |
| $P$ (projection) | Scales into the clip box; orthographic vs perspective lives here | Zoom or window resize |
| $PV$ | Both at once | Rebuild when either changes |

**Implementation notes:**

- Keep $V$ and $P$ separate on the CPU; multiply once per frame and upload $PV$ in a uniform buffer.
- $V$ is identical for orthographic and perspective. Switching projection mode only swaps $P$.
- Zoom → change $w$, $h$. Never change $R$ to zoom an orthographic camera.
- Guard the up hint in top/bottom views to avoid $\vec{f} \times \vec{u} = \vec{0}$.

---

## Unit test values

Use these when hand-writing the matrices. A sign error is hard to see on screen but trivial to catch here.

**Axes** — $\vec{f} = (0, 0, -1)$, hint $(0, 1, 0)$:
- $\vec{r} = (1, 0, 0)$, $\vec{u}' = (0, 1, 0)$
- Hint $(0, 1, 5)$ gives the same $\vec{r}$ and $\vec{u}'$.
- Hint $(0, 0, 1)$ must be caught (zero cross product).

**View** — $\vec{t} = \vec{0}$, $\vec{e} = (0, 0, 1)$, $\vec{p} = (3, -2, 7)$:
- $R = 10$ → $(3, -2, -3)$
- $R = 50$ → $(3, -2, -43)$

**Projection** — $w = h = 5$, $n = 1$, $f = 21$:
- $(3, -2, -3)$ → $(0.6, -0.4, 0.1)$
- $z_{c} = -1$ → depth $0$
- $z_{c} = -21$ → depth $1$