For a general solution we have the following form:
$$
a\frac{d^2y}{dx^2} + b\frac{dy}{dx} + cy = f(x)
$$
For a simple example of a general solution lets have the following:
$$
\frac{d^2y}{dx^2} + 5\frac{dy}{dx} + 6y = 12
$$
If we make $y_p(x) = 2$ it is a constant so the rates of change collapse to zero. This gives the following:
$$
\frac{d^2y}{dx^2} + 5\frac{dy}{dx} + 6y = 0 + 5 \times 0 + 6 \times 2 = 12
$$
We also know that the following homogeneous equation:
$$
\frac{d^2y}{dx^2} + 5\frac{dy}{dx} + 6y = 0
$$
We have the following general solution:
$$
y_c(x) = Ce^{-2x} + De^{-3x}
$$
With superposition we have $y_c(x) + y_p(x)$, we have the following:
$$
y(x) = Ce^{-2x} + De^{-3x} + 2
$$
We can then define some terms. For the following:
$$
a\frac{d^2y}{dx^2} + b\frac{dy}{dx} + cy = f(x)
$$
It's associated homogeneous equation is:
$$
a\frac{d^2y}{dx^2} + b\frac{dy}{dx} + cy = 0
$$
The general solution $y_c(x)$ of the associated homogeneous equation is known as the complementary function for the original inhomogeneous equation.

Any particular solution $y_p(x)$ of the original inhomogeneous equation is referred to as a particular integral for that equation.

A good example is the following equation:
$$
\frac{d^2y}{dx^2} + 9y = 9x + 9
$$
Because of superposition $y(x) = y_c(x) + y_p(x) \implies \lambda^{2} + 0\lambda + 9 = 0$:
$$
\lambda = \frac{0 \pm \sqrt{0^{2} - 4 \times 9}}{2} = \frac{0 \pm \sqrt{-36}}{2} = \frac{0 \pm 6i}{2} = 0 \pm 3i
$$
Remembering Euler's formula we get:
$$
y_c(x) = Ce^{3ix} + De^{-3ix} \implies y_c(x) = C\cos(3x) + D\sin(3x)
$$
For $y_p(x) = x + 1$:
$$
\frac{dy}{dx} = 1 \implies \frac{d^2y}{dx^2} = 0
$$
So:
$$
\frac{d^2y}{dx^2} + 9y = 9(x + 1) = 9x + 9
$$
So:
$$
y(x) = C\cos(3x) + D\sin(3x) + x + 1
$$

## Connections
- The right hand side is no longer zero, so this is the general case of [[linear homogenious equations]] — that note gives the complementary function $y_c(x)$, and all that is left is to find a particular integral $y_p(x)$.
- Superposition is what lets the two halves be added: $y(x) = y_c(x) + y_p(x)$.
- The second example's complementary function comes from the complex-root case, [[Complex conjugate roots]], with more of the same algebra in [[Complex solutions to linear homogenous differential equations]].
- Physically, $f(x)$ is the driving force — the $+f(t)$ term that turns the damped oscillator in [[dampened harmonic oscillator]] into the forced damped harmonic oscillator of [[Second Order differential Equations]].
- Part of [[Differential Equations]].
