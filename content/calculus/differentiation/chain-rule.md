---
tags:
  - reference
  - example
area: calculus
topic: differentiation
---
# The chain rule

How to differentiate a **composite function** — a function inside another function, like
$(x^2+1)^5$ or $\sin(3x)$. You differentiate the outer function and the inner function and
**multiply** the results.

## The formula
If $y = f(g(x))$, then:
$$
\frac{dy}{dx} = f'\big(g(x)\big)\cdot g'(x)
$$
Equivalently, with $u = g(x)$ so that $y = f(u)$:
$$
\frac{dy}{dx} = \frac{dy}{du}\cdot\frac{du}{dx}
$$

## Intuition — rates multiply
The Leibniz form makes it obvious: if $y$ changes twice as fast as $u$, and $u$ changes three
times as fast as $x$, then $y$ changes $2\times 3 = 6$ times as fast as $x$. The $du$'s
"cancel" like fractions (a useful mnemonic, though it's really the limit definition doing the
work).

## The recipe
1. **Spot the layers:** identify the *outer* function and the *inner* function $u = g(x)$.
2. **Differentiate the outer**, leaving the inner untouched: $f'(u)$.
3. **Multiply by the derivative of the inner**, $g'(x)$.
4. (Optional) back-substitute $u = g(x)$.

## Worked example 1 — $\dfrac{d}{dx}(x^2 + 1)^5$
- Inner: $u = x^2 + 1 \implies \dfrac{du}{dx} = 2x$
- Outer: $y = u^5 \implies \dfrac{dy}{du} = 5u^4$

Multiply:
$$
\frac{dy}{dx} = 5u^4 \cdot 2x = 10x\,(x^2+1)^4
$$
$$
\boxed{\,\frac{d}{dx}(x^2+1)^5 = 10x\,(x^2+1)^4\,}
$$

## Worked example 2 — $\dfrac{d}{dx}\sin(3x)$
- Inner: $u = 3x \implies \dfrac{du}{dx} = 3$
- Outer: $\sin u \implies \dfrac{d}{du}\sin u = \cos u$

$$
\frac{d}{dx}\sin(3x) = \cos(3x)\cdot 3 = 3\cos(3x)
$$
This is exactly where the factor of $a$ in $\dfrac{d}{dx}\sin(ax) = a\cos(ax)$ comes from —
and, run backwards, the $\tfrac{1}{a}$ in [[trig|trig integration]].

## Worked example 3 — $\dfrac{d}{dx}e^{x^2}$
- Inner: $u = x^2 \implies \dfrac{du}{dx} = 2x$
- Outer: $e^u \implies \dfrac{d}{du}e^u = e^u$

$$
\frac{d}{dx}e^{x^2} = e^{x^2}\cdot 2x = 2x\,e^{x^2}
$$

## Connections
- Part of [[Calculus]]; built on the limit definition in [[lim-zero-example]].
- The chain rule run **backwards** is [[by-substitution|integration by substitution]].
- Combines with the [[product rule]] — whose reverse is [[by-parts]] — for messier expressions.
- It is the source of every "divide by the inner coefficient" in the integration tables: the $\tfrac1a$ in [[trig]] and in [[exponents]].
- The factor of $a$ on $\sin(ax)$ and $\cos(ax)$ throughout [[compound angle formula]] is this rule.
- Differentiating the trial solution $y = e^{\lambda x}$ in [[linear homogenious equations]] uses it.
