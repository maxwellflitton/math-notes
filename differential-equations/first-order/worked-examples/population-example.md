---
tags:
  - example
area: differential-equations
topic: first-order
---
# Worked example: the logistic population model

Solving the logistic equation from [[First order differential equations]] in full:
$$
\frac{dP}{dt} = k\,P\left(1 - \frac{P}{M}\right)
$$
where $k$ is the growth rate and $M$ is the **carrying capacity** (the population the
resources can sustain). Unlike the simple exponential model, growth slows as $P \to M$.

## 1. Separate the variables
Move all the $P$ terms to one side and the $t$ terms to the other:
$$
\frac{dP}{P\left(1 - \frac{P}{M}\right)} = k\,dt
$$
Tidy the left denominator using $1 - \frac{P}{M} = \frac{M - P}{M}$:
$$
\frac{M}{P\,(M - P)}\,dP = k\,dt
$$

## 2. Split the left side with partial fractions
We want constants $A, B$ such that:
$$
\frac{M}{P\,(M - P)} = \frac{A}{P} + \frac{B}{M - P}
\implies M = A\,(M - P) + B\,P
$$
Choosing convenient values of $P$:
$$
\begin{aligned}
P = 0 \ &\implies\ M = A\,M \implies A = 1 \\[6pt]
P = M \ &\implies\ M = B\,M \implies B = 1
\end{aligned}
$$
So the equation becomes:
$$
\left(\frac{1}{P} + \frac{1}{M - P}\right) dP = k\,dt
$$

## 3. Integrate both sides
$$
\int \frac{1}{P}\,dP + \int \frac{1}{M - P}\,dP = \int k\,dt
$$
The second integral picks up a minus sign because $\frac{d}{dP}(M - P) = -1$:
$$
\ln |P| - \ln |M - P| = kt + C
$$
Combine the logs (see [[exp-algebria-rules]]):
$$
\ln \left|\frac{P}{M - P}\right| = kt + C
$$

## 4. Exponentiate and solve for $P$
$$
\frac{P}{M - P} = e^{kt + C} = A\,e^{kt} \qquad (A = e^{C})
$$
Rearranging for $P$:
$$
\begin{aligned}
P &= A\,e^{kt}\,(M - P) \\[6pt]
P + A\,e^{kt} P &= A\,M\,e^{kt} \\[6pt]
P\left(1 + A\,e^{kt}\right) &= A\,M\,e^{kt} \\[6pt]
P &= \frac{A\,M\,e^{kt}}{1 + A\,e^{kt}}
\end{aligned}
$$

## 5. Apply the initial condition
Let $P_0 = P(0)$. From $\frac{P}{M - P} = A\,e^{kt}$ at $t = 0$:
$$
A = \frac{P_0}{M - P_0}
$$
Substituting back and simplifying (multiply top and bottom by $e^{-kt}$) gives the standard
logistic solution:
$$
\boxed{\,P(t) = \frac{M\,P_0}{P_0 + (M - P_0)\,e^{-kt}}\,}
$$

## Sanity checks
- **$t \to \infty$:** $e^{-kt} \to 0$, so $P(t) \to M$ — the population levels off at the carrying capacity. ✓
- **$t = 0$:** the denominator becomes $P_0 + (M - P_0) = M$, so $P(0) = \frac{M P_0}{M} = P_0$. ✓
- **Small $P_0 \ll M$ early on:** the $e^{-kt}$ term dominates and growth is approximately exponential, matching the simpler model — before resource limits bite. ✓

## Takeaway
The S-shaped (sigmoid) logistic curve is the workhorse for bounded growth. The two new
ingredients beyond the exponential model are **partial fractions** (to integrate the
separated left side) and a **carrying capacity** $M$ that caps the growth.
