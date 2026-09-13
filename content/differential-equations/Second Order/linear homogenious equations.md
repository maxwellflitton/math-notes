A homogeneous differential equation has zero on the right hand side. Take the following:
$$
\frac{d^2y}{dt^2} - \omega^{2}y = 0
$$
Lets say that $y = e^{\lambda t}$, we get:
$$
\frac{dy}{dt} = \lambda e^{\lambda t} \quad \text{and} \quad \frac{d^2y}{dt^2} = \lambda^{2}e^{\lambda t}
$$
This means that the differential equation is:
$$
\lambda^{2}e^{\lambda t} - \omega^{2}e^{\lambda t} = 0
$$
The exponents then cancel out because they are never zero:
$$
\lambda^{2} - \omega^{2} = 0
$$
With this we can see that $\lambda = \omega$ and $\lambda = -\omega$, so $y = e^{\omega t}$ and $y = e^{-\omega t}$. So if we have a homogeneous equation we get the following general solution:
$$
y(t) = Ce^{\omega t} + De^{-\omega t}
$$
For the following:
$$
\frac{d^2y}{dt^2} + \omega^{2}y = 0
$$
We get the following:
$$
\lambda^{2}e^{\lambda t} + \omega^{2}e^{\lambda t} = 0 \implies \lambda^{2} + \omega^{2} = 0
$$
Because we have a square root for negative numbers the solution needs a complex number, so we get the following:
$$
y = e^{i\omega t} \quad \text{and} \quad y = e^{-i\omega t}
$$
So:
$$
y(t) = Ae^{i\omega t} + Be^{-i\omega t}
$$
We can denote that $e^{ix} = \cos(x) + i\sin(x)$ where $x = \omega t$ gives:
$$
e^{i\omega t} = \cos(\omega t) + i\sin(\omega t)
$$
And where $x = -\omega t$ gives:
$$
\begin{aligned}
e^{-i\omega t} &= \cos(-\omega t) + i\sin(-\omega t) \\[6pt]
&= \cos(\omega t) - i\sin(\omega t)
\end{aligned}
$$
We can plug them into:
$$
y(t) = Ae^{i\omega t} + Be^{-i\omega t}
$$
Which gives:
$$
y(t) = C\cos(\omega t) + D\sin(\omega t)
$$
Where $C = A + B$ and $D = i(A - B)$.

We can find a solution in a general case. Suppose we want to solve the following equation:
$$
a\frac{d^2y}{dx^2} + b\frac{dy}{dx} + cy = 0
$$
We then substitute the trial solution $y = e^{\lambda x}$, therefore:
$$
\frac{dy}{dx} = \lambda e^{\lambda x} \quad \text{and} \quad \frac{d^2y}{dx^2} = \lambda^{2}e^{\lambda x}
$$
Which gives us the following:
$$
\begin{aligned}
a\frac{d^2y}{dx^2} + b\frac{dy}{dx} + cy &= a\lambda^{2}e^{\lambda x} + b\lambda e^{\lambda x} + ce^{\lambda x} \\[6pt]
&= (a\lambda^{2} + b\lambda + c)e^{\lambda x}
\end{aligned}
$$
The equation $y = e^{\lambda x}$ is true if the following quadratic equation is satisfied:
$$
a\lambda^{2} + b\lambda + c = 0
$$
Because a quadratic equation has two solutions we can use superposition to have the following:
$$
y(x) = Ce^{\lambda_1 x} + De^{\lambda_2 x}
$$
$$
\lambda_1 = \frac{-b + \sqrt{b^{2} - 4ac}}{2a} \quad \text{and} \quad \lambda_2 = \frac{-b - \sqrt{b^{2} - 4ac}}{2a}
$$
This result is summarised as a theorem.

> [!note] Theorem 1 — General solution of homogeneous equations
> Given a homogeneous linear second-order differential equation
> $$a\frac{d^2y}{dx^2} + b\frac{dy}{dx} + cy = 0,$$
> with constant coefficients $a \neq 0$, `b` and `c`, the auxiliary equation is
> $$a\lambda^{2} + b\lambda + c = 0.$$
> This usually has two distinct roots, $\lambda_1$ and $\lambda_2$, associated with two distinct solutions $y_1(x) = e^{\lambda_1 x}$ and $y_2(x) = e^{\lambda_2 x}$. Provided that the roots are distinct, the general solution of the differential equation is
> $$y(x) = Ce^{\lambda_1 x} + De^{\lambda_2 x},$$
> where `C` and `D` are arbitrary constants.

Assuming that the coefficients `a`, `b` and `c` are real, there are three cases to consider, depending on the sign of the discriminant $b^{2} - 4ac$:

- For $b^{2} - 4ac > 0$, the roots are distinct and real.
- For $b^{2} - 4ac < 0$, the roots are distinct and complex.
- For $b^{2} - 4ac = 0$, the roots are equal and real.

For an example we can take the following:
$$
\frac{d^2y}{dx^2} - 3\frac{dy}{dx} + 2y = 0
$$
We can define the auxiliary equation as $\lambda^{2} - 3\lambda + 2 = 0$ which can be factored into:
$$
(\lambda - 1)(\lambda - 2) = 0
$$
So $\lambda_1 = 1$ and $\lambda_2 = 2$, where $y_1 = e^{x}$ and $y_2 = e^{2x}$:
$$
y(x) = Cy_1(x) + Dy_2(x) = Ce^{x} + De^{2x}
$$
Therefore:
$$
\frac{dy}{dx} = Ce^{x} + 2De^{2x} \implies \frac{d^2y}{dx^2} = Ce^{x} + 4De^{2x}
$$
Substituting into the left hand side we get the following:
$$
\begin{aligned}
\frac{d^2y}{dx^2} - 3\frac{dy}{dx} + 2y &= (Ce^{x} + 4De^{2x}) - 3(Ce^{x} + 2De^{2x}) + 2(Ce^{x} + De^{2x}) \\[6pt]
&= C(1 - 3 + 2)e^{x} + D(4 - 6 + 2)e^{2x} \\[6pt]
&= 0
\end{aligned}
$$

## Connections
- The core theorem of the second-order material introduced in [[Second Order differential Equations]]; part of [[Differential Equations]].
- The three discriminant cases:
	- $b^2 - 4ac > 0$ — distinct real roots, worked above.
	- $b^2 - 4ac < 0$ — distinct complex roots → [[Complex conjugate roots]], with more examples in [[Complex solutions to linear homogenous differential equations]].
	- $b^2 - 4ac = 0$ — equal roots, where Theorem 1 fails and the $(C + Dx)e^{\lambda x}$ fix is needed (also in [[Complex conjugate roots]]).
- Physical reading of the same three cases — under-, over- and critically damped: [[dampened harmonic oscillator]].
- Why $e^{\lambda t}$ is the right guess (it is its own derivative, and never zero, so it cancels): [[exp-algebria-rules]].
- The structural reason the guess works: $e^{\lambda x}$ is an eigenfunction of $\frac{d}{dx}$, so the auxiliary equation is an eigenvalue equation — [[eigen]].
- Differentiating $e^{\lambda t}$ to $\lambda e^{\lambda t}$ is [[chain-rule|the chain rule]].
- Euler's formula $e^{ix} = \cos x + i\sin x$ turns the complex exponentials into the trig pair — the same identity that proves the [[compound angle formula|compound angle formulae]].
