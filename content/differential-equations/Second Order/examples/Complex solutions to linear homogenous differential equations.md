These are worked examples of the $b^{2} - 4ac < 0$ case of [[linear homogenious equations|the general solution of homogeneous equations]], where the roots of the auxiliary equation are distinct and complex.

We can start our example with the following equation:
$$
\frac{d^2\theta}{dt^2} + 9\theta = 0 \implies \lambda^{2} + 0\lambda + 9 = 0
$$
Therefore the auxiliary equation gives the following:
$$
\lambda = \frac{-0 \pm \sqrt{0^{2} - 4 \cdot 1 \cdot 9}}{2} = \frac{0 \pm \sqrt{-36}}{2} = 0 \pm 3i
$$
Here we must note that because we do not have a first derivative term, there are no real numbers for $\lambda$. $\lambda$ is purely imaginary. With the $\lambda$ we remember the following general solution:
$$
\theta(t) = Ce^{\lambda_1 t} + De^{\lambda_2 t} = Ce^{(0 + 3i)t} + De^{(0 - 3i)t}
$$
$$
\theta(t) = Ce^{0}e^{3it} + De^{0}e^{-3it} = Ce^{3it} + De^{-3it}
$$
We then employ Euler's formula for the following solution:
$$
\theta(t) = C\cos(3t) + D\sin(3t)
$$
Here we can see that there is just pure oscillation, zero decay or growth over time. The imaginary part gives the rhythm of the solution. Therefore we can deduce that the real component will not result in $e^{0}$ and thus encode the oscillation in a growth or decay term.

We can show this with the following:
$$
\frac{d^2y}{dx^2} + 4\frac{dy}{dx} + 8y = 0 \implies \lambda^{2} + 4\lambda + 8 = 0
$$
So:
$$
\lambda = \frac{-4 \pm \sqrt{16 - 4 \cdot 8}}{2} = \frac{-4 \pm \sqrt{-16}}{2} = \frac{-4 \pm i4}{2} = -2 \pm 2i
$$
This gives the following:
$$
y(x) = Ce^{(-2 + 2i)x} + De^{(-2 - 2i)x} = Ce^{-2x}e^{2ix} + De^{-2x}e^{-2ix}
$$
$$
y(x) = e^{-2x}\left(De^{-2ix} + Ce^{2ix}\right) = e^{-2x}\left(C\cos(2x) + D\sin(2x)\right)
$$
Here we can see the oscillations get smaller as `x` increases.

## Connections
- Worked examples for the theory in [[Complex conjugate roots]], which is itself the complex case of [[linear homogenious equations]].
- The first example ($\lambda = \pm 3i$, pure oscillation) is the undamped spring; the second ($\lambda = -2 \pm 2i$, decaying oscillation) is underdamped motion — both are treated physically in [[dampened harmonic oscillator]].
- The pattern to take away: the **real** part of $\lambda$ sets growth or decay, the **imaginary** part sets the rhythm.
- Euler's formula step: [[compound angle formula]]. Exponent splitting: [[exp-algebria-rules]].
- Part of [[Differential Equations]]; the equations come from [[Second Order differential Equations]].
