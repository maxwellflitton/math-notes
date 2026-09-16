---
title: Orthographic Projection Application
tags: [computer-graphics, cad, projection, linear-algebra]
created: 2026-09-16
---

# Orthographic Projection Application

Every matrix of the orthographic camera, put together.

---

## 0. Symbols

| Symbol | Meaning |
| --- | --- |
| $\mathbf{p} = (x\ \ y\ \ z\ \ 1)^\top$ | a world point (homogeneous) |
| $\mathbf{t}$ | the target the camera looks at |
| $\mathbf{e}$ | unit direction from the target towards the eye |
| $\mathbf{u}$ | world up vector |
| $d$ | zoom distance, sets the half-height of the view |
| $s$ | scene extent |
| $A$ | aspect ratio (width / height) |
| $W,\ H$ | viewport size in pixels |
| $w = A\,d\tan\frac{\pi}{8}$ | half-width of the view |
| $h = d\tan\frac{\pi}{8}$ | half-height of the view |

---

## 1. The whole pipeline

In column form:

$$\boxed{\ \mathbf{p}_{\text{clip}} = P\,V\,\mathbf{p}\ }$$

The bottom row of both $P$ and $V$ is $(0\ 0\ 0\ 1)$, so the last component of $\mathbf{p}_{\text{clip}}$ is $1$.

This means:

- Normalised device coordinates **are** the clip coordinates, unchanged.
- The perspective divide is a division by one.

That single fact is what makes the projection orthographic.

---

## 2. The view matrix

Rows are the camera axes, the last column is the shift to the eye measured along those axes:

$$
V =
\begin{pmatrix}
r_x & r_y & r_z & -\mathbf{r}\cdot\mathbf{E} \\
u'_x & u'_y & u'_z & -\mathbf{u}'\cdot\mathbf{E} \\
e_x & e_y & e_z & -\mathbf{e}\cdot\mathbf{E} \\
0 & 0 & 0 & 1
\end{pmatrix}
=
\begin{pmatrix}
Q^\top & -Q^\top\mathbf{E} \\
\mathbf{0}^\top & 1
\end{pmatrix},
\qquad
Q = \begin{pmatrix} \mathbf{r} & \mathbf{u}' & \mathbf{e} \end{pmatrix}
$$

$Q$ is the rotation whose **columns** are the camera axes, so $Q^\top$ takes world coordinates to camera coordinates.

Read $V$ as "translate by $-\mathbf{E}$, then rotate by $Q^\top$", since

$$Q^\top(\mathbf{p} - \mathbf{E}) = Q^\top\mathbf{p} - Q^\top\mathbf{E}$$

### Ingredients, in the order they are computed

Eye distance and eye position:

$$R = \max\big(d,\ \max(1.5\,s,\ 10)\big), \qquad \mathbf{E} = \mathbf{t} + R\,\mathbf{e}$$

Camera axes:

$$\mathbf{f} = -\mathbf{e}, \qquad \mathbf{r} = \frac{\mathbf{f}\times\mathbf{u}}{\lVert \mathbf{f}\times\mathbf{u} \rVert}, \qquad \mathbf{u}' = \mathbf{r}\times\mathbf{f}$$

---

## 3. The projection matrix

$$
P =
\begin{pmatrix}
\dfrac{1}{A\,d\tan\frac{\pi}{8}} & 0 & 0 & 0 \\[2ex]
0 & \dfrac{1}{d\tan\frac{\pi}{8}} & 0 & 0 \\[2ex]
0 & 0 & -10^{-4} & 0 \\[1ex]
0 & 0 & 0 & 1
\end{pmatrix}
=
\begin{pmatrix}
1/w & 0 & 0 & 0 \\
0 & 1/h & 0 & 0 \\
0 & 0 & -10^{-4} & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

---

## 4. The product $PV$

$P$ is diagonal, so it just scales the rows of $V$:

$$
PV =
\begin{pmatrix}
r_x/w & r_y/w & r_z/w & -\mathbf{r}\cdot\mathbf{t}\,/\,w \\
u'_x/h & u'_y/h & u'_z/h & -\mathbf{u}'\cdot\mathbf{t}\,/\,h \\
-e_x/10^4 & -e_y/10^4 & -e_z/10^4 & (\mathbf{e}\cdot\mathbf{t} + R)/10^4 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

The last column uses the following (because $\mathbf{E} = \mathbf{t} + R\,\mathbf{e}$ and $\mathbf{r},\mathbf{u}' \perp \mathbf{e}$, $\lVert\mathbf{e}\rVert = 1$):

$$\mathbf{r}\cdot\mathbf{E} = \mathbf{r}\cdot\mathbf{t}, \qquad \mathbf{u}'\cdot\mathbf{E} = \mathbf{u}'\cdot\mathbf{t}, \qquad \mathbf{e}\cdot\mathbf{E} = \mathbf{e}\cdot\mathbf{t} + R$$

Multiplying through by $\mathbf{p}$ gives the three dot products in their final form:

$$
x_{\text{ndc}} = \frac{(\mathbf{p}-\mathbf{t})\cdot\mathbf{r}}{w}, \qquad
y_{\text{ndc}} = \frac{(\mathbf{p}-\mathbf{t})\cdot\mathbf{u}'}{h}, \qquad
z_{\text{ndc}} = \frac{(\mathbf{t}-\mathbf{p})\cdot\mathbf{e} + R}{10^4}
$$

---

## 5. To pixels

The last map is not a $4\times4$ in the engine, but it is affine too:

$$
p_x = \frac{x_{\text{ndc}} + 1}{2}\,W, \qquad
p_y = \frac{1 - y_{\text{ndc}}}{2}\,H
$$

- The flip on $y$ is because pixel rows count downward.
- WebGPU then keeps a fragment only if $0 \le z_{\text{ndc}} \le 1$, which discards everything behind the eye.

---

## 6. Row form, as the engine stores it

The engine writes points as **rows** and multiplies on the **right**, so it holds the transposes and computes

$$\mathbf{p}^\top V^\top P^\top = (P\,V\,\mathbf{p})^\top$$

Same numbers, same order of application, but:

- the axes run down the **columns**,
- the shift runs along the **bottom row**.

> [!note]
> When comparing a matrix in these notes against the source code, transpose it.

---

## 7. Running it backwards

$P$ and $V$ are both invertible, so a screen point with a known depth has exactly one world point:

$$
\mathbf{p} = \mathbf{t} + (x_{\text{ndc}}\,w)\,\mathbf{r} + (y_{\text{ndc}}\,h)\,\mathbf{u}' + \lambda\,\mathbf{e},
\qquad
\lambda = R - 10^4\,z_{\text{ndc}}
$$

Leave $\lambda$ free and this is the **pick line** for that pixel.

- Every pick line points along $-\mathbf{e}$.
- Only where the line starts moves with the pixel.

That parallel family of lines is the orthographic signature, and the reason orthographic picking algorithms are shorter than perspective ones.
