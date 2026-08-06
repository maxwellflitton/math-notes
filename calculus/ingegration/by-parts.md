---
tags:
  - reference
  - example
area: calculus
topic: integration
---
# Integration by parts

A technique for integrating a **product** of two functions — the integration counterpart of
the product rule for differentiation.

## The formula
$$
\int u \, dv = uv - \int v \, du
$$
You pick part of the integrand to be $u$ (which you'll differentiate) and the rest to be
$dv$ (which you'll integrate). The goal: trade the original integral $\int u\,dv$ for an
*easier* one, $\int v\,du$.

## Where it comes from
Start from the product rule and integrate both sides:
$$
\frac{d}{dx}(uv) = u\frac{dv}{dx} + v\frac{du}{dx}
\implies uv = \int u\,dv + \int v\,du
$$
Rearranging for the term you want gives the formula above.

## Choosing $u$ — the LIATE rule
Pick $u$ as whichever type appears **first** in this list (it differentiates toward something
simpler); the rest becomes $dv$:

> **L**ogarithmic → **I**nverse trig → **A**lgebraic (e.g. $x^n$) → **T**rig → **E**xponential

The aim is for $du$ to get simpler and $dv$ to stay easy to integrate.

## Worked example — $\int x\,e^{x}\,dx$
Here $x$ is **A**lgebraic and $e^x$ is **E**xponential, so by LIATE choose $u = x$:

$$
\begin{aligned}
u = x \quad &\implies \quad du = dx \\[6pt]
dv = e^{x}\,dx \quad &\implies \quad v = \int e^{x}\,dx = e^{x}
\end{aligned}
$$

Apply $\int u\,dv = uv - \int v\,du$:
$$
\int x\,e^{x}\,dx = x\,e^{x} - \int e^{x}\,dx = x\,e^{x} - e^{x} + C
$$

Factor:
$$
\boxed{\,\int x\,e^{x}\,dx = e^{x}(x - 1) + C\,}
$$

> [!check] Verify by differentiating back
> $\dfrac{d}{dx}\big[e^{x}(x-1)\big] = e^{x}(x-1) + e^{x} = e^{x}\,x = x\,e^{x}$ ✓ (product rule).

Notice the payoff: the new integral $\int e^x\,dx$ was trivial, whereas the original $\int x\,e^x\,dx$ wasn't — that's the whole point of the swap. Had we picked $u = e^x$ instead, $du$ would have stayed an exponential and the new integral would have been *harder*, which is exactly what LIATE steers you away from.

## A useful trick — $\int \ln x \, dx$
When there's seemingly only one function, set $dv = dx$:
$$
u = \ln x,\quad dv = dx \implies du = \tfrac{1}{x}dx,\quad v = x
$$
$$
\int \ln x\,dx = x\ln x - \int x\cdot\tfrac1x\,dx = x\ln x - x + C
$$

# Simplified Equation
A more straight forward equation for this solution is the following:
$$
\int f(x)g'(x),dx = f(x)g(x) - \int f'(x)g(x),dx
$$
This really nails it. If we have the following:
$$
\frac{dy}{dx} = xe^{-2x}
$$
We can have the following equations:
$$
\begin{aligned}
f(x) &= x        & f'(x) &= 1 \\[6pt]
g'(x) &= e^{-2x} & g(x) &= \frac{-1}{2} e^{-2x}
\end{aligned}
$$
We can the plug this into the equation giving the following:
$$
y = x\frac{-1}{2} e^{-2x} - \int 1\frac{-1}{2} e^{-2x},dx
$$
Which integrates into the following:
$$
y = x\frac{-1}{2} e^{-2x} - \frac{-1}{4} e^{-2x} + C
$$
Which can be simplified to the following:
$$
y = \frac{-1}{2} \left(1 + \frac{1}{2}\right) e^{-2x} + C
$$

## Connections
- The reverse of the product rule — pairs with the chain-rule-based [[trig|trig integration]] and $u$-substitution.
- Exponent/log mechanics used above: [[exp-algebria-rules]].
