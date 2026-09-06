---
tags:
  - reference
  - example
area: calculus
topic: integration
---
# Integration by substitution

The integration counterpart of the **chain rule**. You spot an inner function whose
derivative also appears in the integrand, rename it $u$, and the integral collapses to
something standard.

## The formula
If $u = g(x)$ then $du = g'(x)\,dx$, and:
$$
\int f\big(g(x)\big)\,g'(x)\,dx = \int f(u)\,du
$$
The trick is recognising that the integrand is (a function of $u$) $\times$ ($u$'s derivative).

## Where it comes from
The chain rule says $\dfrac{d}{dx}F\big(g(x)\big) = F'\big(g(x)\big)\,g'(x)$. Integrating both
sides just undoes it:
$$
\int F'\big(g(x)\big)\,g'(x)\,dx = F\big(g(x)\big) + C
$$
Substitution is simply a bookkeeping device for running that backwards.

## The recipe
1. **Choose $u$** — usually the "inside" of a composite function, or whatever has its derivative also present.
2. **Differentiate:** $du = g'(x)\,dx$, then solve for $dx$ (or for the chunk you need).
3. **Substitute** so the integral is entirely in $u$ — no $x$ left.
4. **Integrate** in $u$.
5. **Back-substitute** $u = g(x)$ (for a definite integral, change the limits instead — see below).

## Worked example 1 (indefinite) — $\int 2x\,(x^2 + 1)^5\,dx$
The inside is $x^2 + 1$, and its derivative $2x$ is sitting right there:
$$
u = x^2 + 1 \implies du = 2x\,dx
$$
Substitute — the $2x\,dx$ becomes $du$:
$$
\int 2x\,(x^2+1)^5\,dx = \int u^5\,du = \frac{u^6}{6} + C
$$
Back-substitute:
$$
\boxed{\,\int 2x\,(x^2+1)^5\,dx = \frac{(x^2+1)^6}{6} + C\,}
$$
> [!check] Verify
> $\dfrac{d}{dx}\!\left[\tfrac16 (x^2+1)^6\right] = \tfrac16 \cdot 6(x^2+1)^5 \cdot 2x = 2x(x^2+1)^5$ ✓ (chain rule).

## Worked example 2 (definite) — $\int_{0}^{2} x\,e^{x^2}\,dx$
Here $du = 2x\,dx$ but we only have $x\,dx$, so solve for it: $x\,dx = \tfrac12\,du$.

**Change the limits** to $u$-values (then you never have to back-substitute):
$$
x = 0 \implies u = 0, \qquad x = 2 \implies u = 4
$$
$$
\int_{0}^{2} x\,e^{x^2}\,dx = \frac{1}{2}\int_{0}^{4} e^{u}\,du = \frac{1}{2}\Big[\,e^{u}\,\Big]_{0}^{4} = \frac{1}{2}\big(e^{4} - 1\big)
$$

> [!tip] Missing a constant factor?
> If the integrand has, say, $x\,dx$ but you need $2x\,dx = du$, just carry the compensating $\tfrac12$ outside the integral (as above). You can only fix a missing **constant** this way — never a missing variable like an extra $x$.

# Simplified Example
Below we have the following differential equation:
$$
\frac{dp}{dt} = \frac{t}{1 + t^2}
$$
For substitution we can have the following:
$$
u = 1 + t^2
$$
And therefore the derivative is:
$$
\frac{du}{dt} = 2t
$$
Substituting the functions we have the following differential equation:
$$
\frac{dp}{dt} = \frac{1}{2} \frac{1}{u}\frac{du}{dt} => p = \frac{1}{2} \int \frac{1}{u}\frac{du}{dt} dt
$$
The `1/2` is so we can substitute in the `2t` which is the `du/dt`. We now have the following direct integration:
$$
p = \frac{1}{2} \int \frac{1}{u} du => p = \frac{1}{2} ln|u| + C
$$
We now substitute the u for the initial function and we have the following solution
$$
p = \frac{1}{2} ln|1 + t^2| + C
$$


## Connections
- Part of [[Calculus]]. Reverse of [[chain-rule|the chain rule]]; complements [[by-parts]] (reverse of the [[product rule]]).
- The factor-of-$a$ shortcut for $\int \sin(at)\,dt$ in [[trig]] is just this substitution done in your head — likewise the $\tfrac1a$ on $\int e^{ax}dx$ in [[exponents]].
- Separation of variables is the same manoeuvre applied to an ODE — see [[Direct Integration]] and the $\int\frac{1}{M-P}dP$ step in [[population-example]].
- Exponent/log mechanics used above: [[exp-algebria-rules]].
