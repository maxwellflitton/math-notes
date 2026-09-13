---
tags:
  - moc
area: differential-equations
---
# Differential Equations

Equations relating a function to its own derivatives. Part of [[Home]].

## First order
- [[First order differential equations]] — the three forms, and the birth-rate model that motivates them.
- [[Direct Integration]] — the $\frac{dy}{dx} = f(x)$ case, and separation of variables.
- [[Linear Differential Equations]] — the integrating factor method.
- [[population-example]] — the logistic equation solved end to end (worked example).

## Second order
- [[Second Order differential Equations]] — what they look like, and $F = ma$ as the motivating case.
- [[linear homogenious equations]] — the trial solution $y = e^{\lambda x}$, the auxiliary equation, and Theorem 1.
- [[Complex conjugate roots]] — the $b^2 - 4ac < 0$ case, plus the repeated-root case.
- [[Complex solutions to linear homogenous differential equations]] — complex-root worked examples.
- [[dampened harmonic oscillator]] — under-, over- and critically damped motion (worked example).

## The through-line
1. A homogeneous linear ODE is solved by guessing $y = e^{\lambda x}$ — see [[linear homogenious equations]].
2. That reduces the ODE to a quadratic, so the **discriminant** decides everything:
   - $b^2 - 4ac > 0$ → two real roots → exponential growth/decay.
   - $b^2 - 4ac < 0$ → complex roots → oscillation, via [[Complex conjugate roots]].
   - $b^2 - 4ac = 0$ → repeated root → the $(C + Dx)e^{\lambda x}$ fix.
3. Euler's formula converts the complex exponentials into $\cos$ and $\sin$, and the [[compound angle formula|compound angle formulae]] then collapse those two into one amplitude-and-phase wave.
4. Physically, that discriminant is the damping: [[dampened harmonic oscillator]].

## Tools it depends on
- Integration: [[Direct Integration]] leans on [[exponents]] and [[by-substitution]]; the integrating factor in [[Linear Differential Equations]] is the [[product rule]] read backwards.
- Exponentials and logs: [[exp-algebria-rules]].
- Why the $e^{\lambda x}$ guess is the natural one rather than a trick: [[eigen]].
- Limits: the $\delta \to 0$ step in [[First order differential equations]] is [[lim-zero-example]].

## Related
- [[Calculus]], [[Trigonometry]], [[Home]]
