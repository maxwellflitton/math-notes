When $b^{2} - 4ac < 0$ the $\lambda_1$ and $\lambda_2$ produce complex roots of one another like the following:
$$
\lambda_1 = \alpha + \beta i \quad \text{and} \quad \lambda_2 = \alpha - \beta i
$$
Where $\alpha$ and $\beta$ are real numbers. This means that the general solution takes the following form:
$$
\begin{aligned}
y(x) &= Ae^{(\alpha + \beta i)x} + Be^{(\alpha - \beta i)x} \\[6pt]
&= Ae^{\alpha x}e^{i\beta x} + Be^{\alpha x}e^{-i\beta x} \\[6pt]
&= e^{\alpha x}\left(Ae^{i\beta x} + Be^{-i\beta x}\right)
\end{aligned}
$$
Which we can simplify using Euler's formula, giving us the following:
$$
y(x) = e^{\alpha x}\left(C\cos(\beta x) + D\sin(\beta x)\right)
$$
Where $C = A + B$ and $D = i(A - B)$.

Below we have an example:
$$
\frac{d^2y}{dx^2} - 6\frac{dy}{dx} + 13y = 0 \implies \lambda^{2} - 6\lambda + 13 = 0
$$
$$
\lambda = \frac{6 \pm \sqrt{36 - 4 \times 1 \times 13}}{2} = \frac{6 \pm \sqrt{-16}}{2} = 3 \pm 2i
$$
$$
y(x) = e^{3x}\left(C\cos(2x) + D\sin(2x)\right)
$$
More examples of this case are in [[Complex solutions to linear homogenous differential equations|the complex solutions worked examples]].

[[linear homogenious equations|Theorem one]] does not work when two roots of the auxiliary are equal. We can show how to solve this with the following equation:
$$
\frac{d^2y}{dx^2} + 2\frac{dy}{dx} + y = 0
$$
Therefore:
$$
\lambda^{2} + 2\lambda + 1 = 0 \implies (\lambda + 1)(\lambda + 1) = 0 \implies (\lambda + 1)^{2} = 0
$$
So $\lambda_1 = \lambda_2 = -1$. Therefore the solution is:
$$
y(x) = Ce^{-x} + De^{-x} = (C + D)e^{-x}
$$
However, we cannot accept that these variables are not independent, so we have the following:
$$
y(x) = v(x)e^{-x}
$$
Where:
$$
v'' = 0 \implies v' = D \implies v(x) = (C + Dx)
$$
So:
$$
y(x) = v(x)e^{-x} = (C + Dx)e^{-x}
$$

## Connections
- Covers two of the three discriminant cases of [[linear homogenious equations]]: complex roots ($b^2 - 4ac < 0$) and repeated roots ($b^2 - 4ac = 0$).
- Euler's formula is what converts $Ae^{i\beta x} + Be^{-i\beta x}$ into $C\cos\beta x + D\sin\beta x$ — the same identity derived in [[compound angle formula]], which also shows how to collapse that pair into one shifted wave.
- Reading the answer physically: $e^{\alpha x}$ is the growth/decay envelope and $\beta$ the oscillation frequency — see [[dampened harmonic oscillator]].
- The equations being solved come from [[Second Order differential Equations]].
- Splitting $e^{(\alpha + \beta i)x}$ into $e^{\alpha x}e^{i\beta x}$ is just the index laws in [[exp-algebria-rules]].
- Complex numbers as a set: [[sets]]. Part of [[Differential Equations]].
