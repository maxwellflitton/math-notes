> [!abstract] One line
> An **eigen**-thing is an object that a linear operator only *scales* — it comes back unchanged in direction or shape, multiplied by a number.

*Eigen* is German for "own" / "characteristic of". An eigenvector is the matrix's **own** vector — the direction it doesn't interfere with.

---

## The defining equation

For a linear operator $L$:

$$
L(v) = \lambda v
$$

- $v \neq 0$ is the **eigenvector** (or **eigenfunction**) — the thing that survives intact.
- $\lambda$ is the **eigenvalue** — the scale factor applied to it.

Everything hinges on that equality: the output is the *same object* as the input, just resized. Any other vector gets rotated, sheared, or bent into something new.

---

## Case 1 — matrices

$$
A\mathbf{v} = \lambda \mathbf{v}
$$

The matrix $A$ stretches $\mathbf{v}$ by $\lambda$ but does not rotate it.

- $\lambda > 1$ — stretch
- $0 < \lambda < 1$ — compress
- $\lambda < 0$ — flip along the same axis
- $\lambda = 0$ — collapse onto zero (the vector is in the null space)

Eigenvectors pick out the **special axes hiding inside a transformation**. A matrix that stretches some directions and rotates the rest is where they earn their keep.

> [!note] The identity is the degenerate case
> For $I$, *every* non-zero vector satisfies $I\mathbf{v} = 1\cdot\mathbf{v}$. So the identity has eigenvectors everywhere with $\lambda = 1$ — which means it isn't specially eigen, it's *trivially* eigen. No direction is singled out because none is treated differently.

→ see [[Computing Eigenvalues and Eigenvectors]], [[Characteristic Polynomial]], [[Diagonalisation]]

---

## Case 2 — functions

Differentiation is also a linear operator; it just acts on functions instead of arrows. So ask the same question: **which function does $\frac{d}{dx}$ only scale?**

$$
\frac{d}{dx}\,e^{kx} = k\,e^{kx}
$$

The function comes back unchanged, multiplied by $k$. So:

- $e^{kx}$ is an **eigenfunction** of $\frac{d}{dx}$
- $k$ is its **eigenvalue**

Plain $e^{x}$ is just the $k = 1$ case — the eigenfunction whose eigenvalue is $1$. That is the whole reason $e$ is the base that matters in analysis.

### Why this explains the ODE ansatz

When solving a linear constant-coefficient ODE, the standard move is to guess $y = e^{\lambda x}$. That is not a trick — you are **hunting for the operator's eigenfunctions**. Substituting turns the differential equation into an algebraic one:

$$
ay'' + by' + cy = 0 \quad \xrightarrow{\;y = e^{\lambda x}\;} \quad a\lambda^{2} + b\lambda + c = 0
$$

The characteristic polynomial *is* the eigenvalue equation in disguise. Differentiation has been replaced by multiplication by $\lambda$.

→ see [[linear homogenious equations]], [[Second Order differential Equations]]

---

## What eigen-decomposition actually buys you

> [!warning] Direction of the implication
> Eigenthings **do not make a system linear**. The system must be linear *first* — a nonlinear operator has no eigenvectors in this sense. Linearity is the precondition, not the prize.

What they give you instead is **decomposition**. Along each eigendirection the operator degenerates into multiplication by a single number. So a coupled, messy $n$-dimensional problem splits into $n$ independent scalar problems:

$$
\mathbf{x} = c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \dots + c_n\mathbf{v}_n
\quad\Longrightarrow\quad
A\mathbf{x} = c_1\lambda_1\mathbf{v}_1 + c_2\lambda_2\mathbf{v}_2 + \dots + c_n\lambda_n\mathbf{v}_n
$$

Solve each piece separately, then glue them back together. **Superposition** is what makes the gluing legal — and superposition is available precisely because the system is linear.

This is the same structural move in both cases above: find the directions the operator leaves alone, work in those coordinates, reassemble.

---

## Connections
- Belongs with [[Linear Algebra]] — a matrix is the linear operator, and the $\mathbf{v}$ it scales is a vector in the sense of [[Basic Operations]], living in [[Euclidean space]].
- Only the *direction* of an eigenvector is fixed; any scalar multiple is still one, which is why they are usually normalised — see [[Unit vectors]].
- Case 2 is the whole reason $y = e^{\lambda x}$ is the trial solution in [[linear homogenious equations]]: the auxiliary equation $a\lambda^2 + b\lambda + c = 0$ *is* the eigenvalue equation, introduced in [[Second Order differential Equations]].
- Complex eigenvalues are the oscillating case — the real part is growth or decay and the imaginary part the rhythm: [[Complex conjugate roots]], read physically in [[dampened harmonic oscillator]].
- $\frac{d}{dx}e^{kx} = ke^{kx}$, the property that makes $e^{kx}$ an eigenfunction, is the one in [[exp-algebria-rules]]; differentiating it is [[chain-rule|the chain rule]].
- Superposition — splitting a problem into independent pieces and gluing them back — is the same move that adds a complementary function to a particular integral in [[linear Inhomogernious equations]].
- Splitting a vector into a part along a direction and a part perpendicular to it is projection — [[Scalar Product]]; an orthonormal eigenbasis is the case where those coordinates cost one dot product each.
- Part of [[Home]].

## Open
- [ ] [[Computing Eigenvalues and Eigenvectors]], [[Characteristic Polynomial]], [[Diagonalisation]] — the matrix machinery referenced above is not written yet.
