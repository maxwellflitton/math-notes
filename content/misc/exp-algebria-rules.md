---
tags:
  - reference
area: misc
---
# Exponent / index algebra rules

Quick reference for manipulating powers and exponential functions. For all rules below
$a, b > 0$ are bases and $m, n$ are real exponents (unless noted).

## Core index laws
| Rule | Identity | Note |
|---|---|---|
| Product (same base) | $a^m \cdot a^n = a^{m+n}$ | multiply → **add** exponents |
| Quotient (same base) | $\dfrac{a^m}{a^n} = a^{m-n}$ | divide → **subtract** exponents |
| Power of a power | $\left(a^m\right)^n = a^{mn}$ | nest → **multiply** exponents |
| Power of a product | $(ab)^n = a^n b^n$ | distributes over $\times$ |
| Power of a quotient | $\left(\dfrac{a}{b}\right)^n = \dfrac{a^n}{b^n}$ | distributes over $\div$ |

> [!warning] These do **not** distribute over $+$
> $a^m + a^n \neq a^{m+n}$ and $(a+b)^n \neq a^n + b^n$. There is no simplification for a sum of powers, or a power of a sum (that's the binomial theorem, not an index law).

## Special exponents
| Rule | Identity | Note |
|---|---|---|
| Zero | $a^0 = 1$ | for any $a \neq 0$ |
| One | $a^1 = a$ | |
| Negative | $a^{-n} = \dfrac{1}{a^n}$ | negative exponent → reciprocal |
| Unit fraction | $a^{1/n} = \sqrt[n]{a}$ | the $n$-th root |
| General fraction | $a^{m/n} = \sqrt[n]{a^m} = \left(\sqrt[n]{a}\right)^m$ | root and power in either order |

## The exponential function $e^x$
Same index laws apply with base $e$ (or any fixed base). Worth memorising on their own:

$$
e^a e^b = e^{a+b}, \qquad
\frac{e^a}{e^b} = e^{a-b}, \qquad
\left(e^a\right)^b = e^{ab}, \qquad
e^0 = 1, \qquad
e^{-a} = \frac{1}{e^a}
$$

Converting between bases (useful when a problem mixes $a^x$ and $e^x$):
$$
a^x = e^{x \ln a}
$$

## Connection to logarithms (the inverse)
The log undoes the exponential — $\log_a$ is the inverse of $a^{(\cdot)}$:
$$
a^x = y \iff \log_a y = x, \qquad
e^x = y \iff \ln y = x
$$
So they cancel:
$$
\ln\!\left(e^x\right) = x, \qquad e^{\ln x} = x \ (x>0)
$$
The index laws become the **log laws** by taking logs of each:
$$
\ln(xy) = \ln x + \ln y, \qquad
\ln\!\frac{x}{y} = \ln x - \ln y, \qquad
\ln\!\left(x^n\right) = n \ln x
$$

## Why this matters for differential equations
The exponential is the function equal to its own derivative, $\dfrac{d}{dx}e^x = e^x$, which is
why it's the natural solution to growth/decay equations like $\dfrac{dp}{dt} = (b-c)\,p(t)$ —
giving $p(t) = p_0\,e^{(b-c)t}$. The base-conversion $a^x = e^{x\ln a}$ is what lets you
differentiate any exponential. See [[First order differential equations]].

That same property is why $y = e^{\lambda x}$ is the trial solution for second-order equations
in [[linear homogenious equations]], and why splitting $e^{(\alpha + \beta i)x} = e^{\alpha x}e^{i\beta x}$
(just the product law above) separates decay from oscillation in [[Complex conjugate roots]].

## Connections
- Part of [[Foundations]], alongside [[sets]].
- Integration tables that apply these rules directly: [[exponents]] (including the $\int x^{-1} = \ln|x|$ gap) and [[trig]].
- Every ODE note leans on the log laws to exponentiate a solution: [[Direct Integration]], [[Linear Differential Equations]], [[population-example]].
- Chosen as $u$ or $dv$ in worked integrals: [[by-parts]], [[by-substitution]].
- The product law is what makes shifting compose, $(x \ll a) \ll b = x \ll (a+b)$, and $\lfloor \log_2 N \rfloor + 1$ is the width of a number in bits — [[Binary Numbers]], applied in [[Bit Encoding]].
- $\frac{d}{dx}e^{kx} = ke^{kx}$ says $e^{kx}$ is an eigenfunction of differentiation, with eigenvalue $k$ — [[eigen]].
- $e^{i\theta} = \cos\theta + i\sin\theta$ gives the one-line proof in [[compound angle formula]].
