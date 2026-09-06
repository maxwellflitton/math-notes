Second order differential equations are where the second order derivative is in the equation. A second order differential equation can take forms like the following:
$$
\frac{d^2y}{dx^2} - 3\frac{dy}{dx} + 2y = 4e^{x} \quad \text{and} \quad 3\frac{d^2y}{dx^2} + y = x\sin(x)
$$
We can denote $F = ma$ with the following:
$$
m\frac{d^2x}{dt^2} = F
$$
With this we can work out more about the system. For instance, lets look at a spring with a weight that rests at $x = 0$. We can say that the force is proportional to the displacement of `x` and we get the following:
$$
F = -kx
$$
Which gives us the following equation:
$$
m\frac{d^2x}{dt^2} = -kx \implies \frac{d^2x}{dt^2} = -\frac{k}{m}x
$$
If $k > 0$ and $m > 0$ then $\omega = \sqrt{k/m}$, so:
$$
\frac{d^2x}{dt^2} = -\omega^{2}x
$$
For now we will just define $x(t)$ and explore later, but for now we just have the following:
$$
x(t) = C\sin(\omega t) + D\cos(\omega t)
$$
$$
\frac{dx}{dt} = C\omega\cos(\omega t) - D\omega\sin(\omega t)
$$
$$
\frac{d^2x}{dt^2} = -C\omega^{2}\sin(\omega t) - D\omega^{2}\cos(\omega t) = -\omega^{2}x
$$
We can add a damper with the following:
$$
m\frac{d^2x}{dt^2} = -kx - \gamma\frac{dx}{dt}
$$
If we want to add another force we get the following:
$$
m\frac{d^2x}{dt^2} = -kx - \gamma\frac{dx}{dt} + f(t)
$$
This is known as a forced damped harmonic oscillator.

If we are going to use direct integration for an equation like the following:
$$
\frac{d^2y}{dt^2} = a
$$
We need two separate arbitrary constants and we need more information. Lets say that we have an equation for position. We would need a position at a point in time and a velocity at a point in time.

## Connections
- The entry point for the second-order half of [[Differential Equations]]; the general method is [[linear homogenious equations]].
- The spring equation $\ddot{x} = -\omega^2 x$ above is the undamped case of [[dampened harmonic oscillator]]; adding $\gamma\frac{dx}{dt}$ and $f(t)$ gives the forced damped oscillator.
- Its oscillating solutions come out of the complex-root case, [[Complex conjugate roots]], with worked examples in [[Complex solutions to linear homogenous differential equations]].
- $C\sin(\omega t) + D\cos(\omega t)$ collapses to a single amplitude-and-phase wave via [[compound angle formula]].
- One order down: [[First order differential equations]]. Direct integration with one constant: [[Direct Integration]].
- Differentiating the trial solution above uses [[chain-rule|the chain rule]].
