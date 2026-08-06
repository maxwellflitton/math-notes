---
tags:
  - reference
  - example
area: calculus
topic: integration
---
# Integrating powers and exponentials

Quick reference for integrals of $x^n$ (powers) and $e^x$ / $a^x$ (exponentials). Every result
has an implied $+\,C$. The algebra of the exponents themselves lives in [[exp-algebria-rules]].

## Powers of $x$ — the power rule
$$
\int x^{n}\,dx = \frac{x^{n+1}}{n+1} \qquad (n \neq -1)
$$
**Add one to the exponent, then divide by the new exponent.** This works for any real $n$
except $-1$ (which would divide by zero — that case is the log below).

| Integral | Result | Note |
|---|---|---|
| $\int x^{n}\,dx$ | $\dfrac{x^{n+1}}{n+1}$ | $n \neq -1$ |
| $\int x^{-1}\,dx = \int \dfrac{1}{x}\,dx$ | $\ln\lvert x\rvert$ | the exceptional case |
| $\int \sqrt{x}\,dx = \int x^{1/2}\,dx$ | $\dfrac{2}{3}x^{3/2}$ | fractional power |
| $\int \dfrac{1}{x^{2}}\,dx = \int x^{-2}\,dx$ | $-\dfrac{1}{x}$ | negative power |
| $\int 1\,dx = \int x^{0}\,dx$ | $x$ | constant case |

> [!warning] The $n = -1$ gap
> $\int x^{-1}\,dx \neq \dfrac{x^{0}}{0}$ — that's undefined. Instead $\int \frac{1}{x}\,dx = \ln\lvert x\rvert + C$. This is the one place the power rule breaks.

## Exponential functions
| Integral | Result | Note |
|---|---|---|
| $\int e^{x}\,dx$ | $e^{x}$ | the function that integrates to itself |
| $\int e^{ax}\,dx$ | $\dfrac{1}{a}e^{ax}$ | divide by the inner coefficient |
| $\int e^{ax+b}\,dx$ | $\dfrac{1}{a}e^{ax+b}$ | constant $b$ rides along |
| $\int a^{x}\,dx$ | $\dfrac{a^{x}}{\ln a}$ | any base $a>0,\ a\neq 1$ |

> [!tip] Where the $\frac{1}{a}$ comes from
> Differentiating $e^{ax}$ gives $a\,e^{ax}$ (chain rule), so integrating must *divide* by $a$ to compensate. Same reasoning as the $\tfrac{1}{a}$ for $\sin(at)$ in [[trig]].

## Linear-inside power rule
A power of a **linear** inner function follows the same divide-by-the-coefficient pattern:
$$
\int (ax+b)^{n}\,dx = \frac{(ax+b)^{n+1}}{a\,(n+1)} \qquad (n \neq -1)
$$

## Worked example 1 — $\int 3x^{2} + \dfrac{1}{x}\,dx$
Integrate term by term (constants pull out, sums split):
$$
\int 3x^{2}\,dx + \int \frac{1}{x}\,dx = 3\cdot\frac{x^{3}}{3} + \ln\lvert x\rvert + C = x^{3} + \ln\lvert x\rvert + C
$$
> [!check] Verify
> $\dfrac{d}{dx}\big(x^{3} + \ln\lvert x\rvert\big) = 3x^{2} + \dfrac{1}{x}$ ✓

## Worked example 2 — $\int e^{2x}\,dx$
Using $\int e^{ax}\,dx = \tfrac{1}{a}e^{ax}$ with $a = 2$:
$$
\boxed{\,\int e^{2x}\,dx = \frac{1}{2}e^{2x} + C\,}
$$
> [!check] Verify
> $\dfrac{d}{dx}\!\left(\tfrac12 e^{2x}\right) = \tfrac12\cdot 2\,e^{2x} = e^{2x}$ ✓ — the chain-rule factor of $2$ cancels the $\tfrac12$.

## Worked example 3 — $\int 2^{x}\,dx$
Any constant base uses $\int a^{x}\,dx = \dfrac{a^{x}}{\ln a}$:
$$
\int 2^{x}\,dx = \frac{2^{x}}{\ln 2} + C
$$
(You can also derive this by writing $2^{x} = e^{x\ln 2}$ — see [[exp-algebria-rules]] — and integrating with $a = \ln 2$.)

## Connections
- Reverse of the [[chain-rule|chain rule]]; the linear-inside cases are quick [[by-substitution]].
- Sits alongside [[trig|trig integration]] and [[by-parts]] as the core integration toolkit.
- Exponent/log algebra: [[exp-algebria-rules]].
