
Here we explain how differentiation happens when the limit trends to zero. First, we have the following equation:
$$
\delta x = x(y + \delta y) - x(y)
$$
Dividing by $\delta y$ gives us the following:
$$
\frac{\delta x}{\delta y} = \frac{x(y + \delta y) - x(y)}{\delta y}
$$
When $\delta y$ trends to zero we get the following:
$$
\frac{dx}{dy} = \lim_{\delta y \to 0} \frac{x(y + \delta y) - x(y)}{\delta y}
$$

## Worked example

Applying this definition to $x(y) = y^2$:
$$
\frac{dx}{dy} = \lim_{\delta y \to 0} \frac{(y + \delta y)^2 - y^2}{\delta y}
$$
Expanding the numerator:
$$
= \lim_{\delta y \to 0} \frac{y^2 + 2y\,\delta y + \delta y^2 - y^2}{\delta y}
= \lim_{\delta y \to 0} \frac{2y\,\delta y + \delta y^2}{\delta y}
$$
Cancelling the $\delta y$:
$$
= \lim_{\delta y \to 0} \big(2y + \delta y\big) = 2y
$$
The $\delta y$ term vanishes in the limit, leaving $\frac{dx}{dy} = 2y$.

## Why it doesn't blow up

A shrinking denominator only explodes to infinity if the numerator stays fixed (e.g. $\lim_{\delta y \to 0} \frac{5}{\delta y} = \infty$). Here the numerator goes to zero *as well*, so we have the **indeterminate form** $\tfrac{0}{0}$ — "small ÷ small" depends entirely on how fast each side shrinks, and isn't automatically infinite.

The resolution is that every term in the numerator carries a factor of $\delta y$, so it cancels:
$$
\frac{2y\,\delta y + \delta y^2}{\delta y} = \frac{\delta y\,(2y + \delta y)}{\delta y} = 2y + \delta y
$$
This cancellation is legal because $\delta y$ is *approaching* zero but never *equals* zero — it is a small nonzero number, so dividing by it is fine. A limit asks what value the expression heads towards, not its value *at* $\delta y = 0$. So we simplify first (while $\delta y \neq 0$), then take the limit of the harmless expression $2y + \delta y \to 2y$.

Intuitively: numerator and denominator both vanish, but in lockstep. The denominator's drive to infinity is exactly matched by the numerator's drive to zero, and they balance at the finite value $2y$ — which is precisely what the derivative measures.

## Connections
- The foundation of [[Calculus]] — every rule above it ([[product rule]], [[chain-rule]]) is a shortcut for this limit.
- The same $\delta \to 0$ argument turns the discrete birth/death difference into a derivative in [[First order differential equations]].
- Reversing these derivatives is what the integration tables in [[trig]] and [[exponents]] are reading off.
- The derivative generalises the constant slope $m$ of [[Lines]] to curves.