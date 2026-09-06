We start with the following equation:
$$
m\frac{d^2x}{dt^2} + \gamma\frac{dx}{dt} + kx = 0
$$
Where `m` and $\gamma$ are positive constants. If we start with $\gamma = 0$ we have no dampening because there are only imaginary numbers, so we have the following:
$$
\frac{d^2x}{dt^2} + \omega^{2}x = 0 \quad \text{where} \quad \omega = \sqrt{\frac{k}{m}}
$$
Giving us $\lambda^{2} + 0\lambda + \omega^{2} = 0$:
$$
\lambda = \frac{-0 \pm \sqrt{-4\omega^{2}}}{2} = 0 \pm i\omega
$$
This gives us:
$$
x(t) = Ce^{i\omega t} + De^{-i\omega t} = C\cos(\omega t) + D\sin(\omega t)
$$
We now have $C = A\sin\theta$ and $D = A\cos\theta$ giving:
$$
\begin{aligned}
x(t) &= (A\sin\theta)\cos(\omega t) + (A\cos\theta)\sin(\omega t) \\[6pt]
&= A\left(\sin\theta\cos(\omega t) + \cos\theta\sin(\omega t)\right)
\end{aligned}
$$
The compound angle formula is:
$$
\sin(P)\cos(Q) + \cos(P)\sin(Q) = \sin(P + Q)
$$
For our equation $P = \theta$ and $Q = \omega t$ so:
$$
x(t) = A\sin(\theta + \omega t)
$$
Now we can apply a dampening parameter with the following:
$$
\frac{d^2x}{dt^2} + 2\Gamma\frac{dx}{dt} + \omega^{2}x = 0
$$
$$
\Gamma = \frac{\gamma}{2m} \quad \omega = \sqrt{\frac{k}{m}}
$$
Our auxiliary equation is $\lambda^{2} + 2\Gamma\lambda + \omega^{2} = 0$, so our:
$$
\lambda = -\Gamma \pm \sqrt{\Gamma^{2} - \omega^{2}}
$$
Here we can see that $\omega$ has to be larger than the dampening factor to get an imaginary root and thus the oscillation. Increasing the mass reduces the $\omega$ but not at the same rate as the $\Gamma$. For underdamped motion:
$$
\Gamma < \omega \implies \lambda = -\Gamma \pm i\Omega \quad \text{where} \quad \Omega = \sqrt{\omega^{2} - \Gamma^{2}}
$$
So:
$$
x(t) = e^{-\Gamma t}\left(C\cos(\Omega t) + D\sin(\Omega t)\right)
$$
$$
x(t) = Ae^{-\Gamma t}\sin(\Omega t + \theta)
$$
Where `A` and $\theta$ are arbitrary constants.

For overdamped motion $\Gamma > \omega$ giving a positive square root and thus no oscillation because we have the following general solution:
$$
x(t) = Ce^{\lambda_1 t} + De^{\lambda_2 t}
$$
For critically damped $\Gamma = \omega$ giving the following:
$$
x(t) = (C + Dt)e^{-\Gamma t}
$$
With this there is no oscillation but there is a bumpy road.

## Connections
- The physical payoff of [[linear homogenious equations]] — the three damping regimes here *are* the three discriminant cases of the auxiliary equation:

| Regime | Condition | Roots | Solution |
|---|---|---|---|
| Undamped | $\Gamma = 0$ | purely imaginary | $A\sin(\omega t + \theta)$ |
| Underdamped | $\Gamma < \omega$ | complex, negative real part | $Ae^{-\Gamma t}\sin(\Omega t + \theta)$ |
| Critically damped | $\Gamma = \omega$ | repeated real | $(C + Dt)e^{-\Gamma t}$ |
| Overdamped | $\Gamma > \omega$ | distinct real | $Ce^{\lambda_1 t} + De^{\lambda_2 t}$ |

- The spring/damper setup and $F = ma$ come from [[Second Order differential Equations]].
- The step collapsing $C\cos\omega t + D\sin\omega t$ into $A\sin(\theta + \omega t)$ is the $R$-formula — derived in full in [[compound angle formula]] (Worked example 5).
- The complex-root algebra behind the underdamped case: [[Complex conjugate roots]] and [[Complex solutions to linear homogenous differential equations]].
- Exponential decay envelope $e^{-\Gamma t}$: [[exp-algebria-rules]].
- Part of [[Differential Equations]].
