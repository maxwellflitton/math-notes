---
tags:
  - reference
  - example
area: calculus
topic: integration
---
# Integrating trig functions

Quick-reference table plus the *why* — how integrating $\sin$ turns into $-\cos$. Every result
below has an implied $+\,C$ (constant of integration).

## Reference table
| Integral | Result | Note |
|---|---|---|
| $\int \sin x \, dx$ | $-\cos x$ | sign flips |
| $\int \cos x \, dx$ | $\sin x$ | no sign flip |
| $\int \sin(at) \, dt$ | $-\dfrac{1}{a}\cos(at)$ | e.g. $\int\sin(2t)\,dt = -\tfrac12\cos(2t)$ |
| $\int \cos(at) \, dt$ | $\dfrac{1}{a}\sin(at)$ | e.g. $\int\cos(2t)\,dt = \tfrac12\sin(2t)$ |
| $\int \sec^2 x \, dx$ | $\tan x$ | |
| $\int \csc^2 x \, dx$ | $-\cot x$ | |
| $\int \sec x \tan x \, dx$ | $\sec x$ | |
| $\int \csc x \cot x \, dx$ | $-\csc x$ | |
| $\int \tan x \, dx$ | $-\ln\lvert\cos x\rvert = \ln\lvert\sec x\rvert$ | |

> [!tip] The $\frac{1}{a}$ comes from the chain rule
> Whenever the angle is $at$ instead of $t$, integrating divides by $a$ (and differentiating multiplies by $a$). This is the source of the $\tfrac12$ in $\int\sin(2t)\,dt$.

## Why $\int \sin$ becomes $-\cos$: the derivative cycle
Integration is just differentiation run backwards, so start from the four derivatives, which
form a repeating cycle:

$$
\sin x \;\overset{\frac{d}{dx}}{\longrightarrow}\; \cos x \;\overset{\frac{d}{dx}}{\longrightarrow}\; -\sin x \;\overset{\frac{d}{dx}}{\longrightarrow}\; -\cos x \;\overset{\frac{d}{dx}}{\longrightarrow}\; \sin x
$$

Integrating walks the **same loop in reverse**. To find $\int \sin x\,dx$ we ask "what
differentiates *to* $\sin x$?" — reading the cycle backwards, that's $-\cos x$:

$$
-\cos x \;\overset{\frac{d}{dx}}{\longrightarrow}\; \sin x
\qquad\Longrightarrow\qquad
\int \sin x \, dx = -\cos x + C
$$

### Seeing it on the unit circle
A point moving anticlockwise around the unit circle has height $\sin\theta$ and horizontal
position $\cos\theta$:

![Unit circle showing sin and cos as coordinates of a moving point](_attachments/unit-circle-sincos.svg)

As $\theta$ increases, the **rate of change** of the height ($\sin$) is governed by the
horizontal position ($\cos$), and vice-versa with a sign flip. That 90° phase relationship
*is* the derivative cycle above — each function's slope is the next one a quarter-turn ahead.

## Worked example 1 — $\int \sin x\, dx$ by verification
The honest way to trust an antiderivative is to differentiate it back:
$$
\frac{d}{dx}\big(-\cos x\big) = -(-\sin x) = \sin x \;\checkmark
$$
The two sign flips — one from the $\frac{d}{dx}\cos = -\sin$ rule, one from the leading minus —
cancel, recovering $\sin x$. Hence $\int \sin x\,dx = -\cos x + C$.

## Worked example 2 — $\int \sin(2t)\, dt$ by substitution
Let $u = 2t$, so $\dfrac{du}{dt} = 2 \implies dt = \dfrac{du}{2}$:
$$
\int \sin(2t)\,dt = \int \sin(u)\,\frac{du}{2} = \frac{1}{2}\int \sin(u)\,du = -\frac{1}{2}\cos(u) + C
$$
Substituting $u = 2t$ back:
$$
\boxed{\,\int \sin(2t)\,dt = -\frac{1}{2}\cos(2t) + C\,}
$$
> [!check] Verify
> $\dfrac{d}{dt}\!\left(-\tfrac12\cos 2t\right) = -\tfrac12 \cdot (-\sin 2t)\cdot 2 = \sin 2t$ ✓ — the chain-rule factor of $2$ cancels the $\tfrac12$.

## Connections
- Part of [[Calculus]]. Reverse of the derivative rules — see [[lim-zero-example]] for differentiation from first principles.
- The substitution in example 2 is the general $u$-substitution technique, [[by-substitution]]; the $\tfrac1a$ it produces is [[chain-rule|the chain rule]] running backwards, exactly as with $\int e^{ax}dx$ in [[exponents]].
- There is no rule here for $\int\sin^2 x\,dx$ — you must first flatten the power with the identities in [[compound angle formula]], which also handles products of trig functions via product-to-sum (avoiding [[by-parts]]).
- These integrals are what you need when solving oscillating systems: [[Second Order differential Equations]] and [[dampened harmonic oscillator]].
- Exponent/log mechanics used alongside these: [[exp-algebria-rules]].

## Reference figure
![[unit_circle_lines.png]]
