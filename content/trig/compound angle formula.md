---
tags:
  - reference
  - concept
  - example
area: trigonometry
topic: identities
---
# Compound angle formulae

> [!summary] In a sentence
> The compound angle (or *addition*) formulae tell you how to split $\sin$, $\cos$ and $\tan$ of a **sum or difference of two angles** into an expression built only from the sines and cosines of the individual angles.

The thing to burn in first: trig functions are **not** linear.
$$
\sin(A+B) \;\neq\; \sin A + \sin B
$$
Quick sanity check with $A = B = 90°$: the left side is $\sin 180° = 0$, the right side is $1 + 1 = 2$. So we need a proper rule — that rule is below.

## The formulae

$$
\boxed{
\begin{aligned}
\sin(A+B) &= \sin A\cos B + \cos A\sin B \\
\sin(A-B) &= \sin A\cos B - \cos A\sin B \\[4pt]
\cos(A+B) &= \cos A\cos B - \sin A\sin B \\
\cos(A-B) &= \cos A\cos B + \sin A\sin B \\[4pt]
\tan(A+B) &= \frac{\tan A + \tan B}{1 - \tan A\tan B} \\
\tan(A-B) &= \frac{\tan A - \tan B}{1 + \tan A\tan B}
\end{aligned}}
$$

> [!tip] How to remember the signs
> - **$\sin$ keeps the sign**, and the terms are *mixed* ($\sin\cos + \cos\sin$).
> - **$\cos$ flips the sign**, and the terms are *matched* ($\cos\cos - \sin\sin$).
> - **$\tan$** has the *same* sign on top and the *opposite* sign on the bottom.
>
> Mnemonic for cosine: "**c**osine is **c**ontrary" — a plus on the left becomes a minus on the right.

---

## Derivation 1 — the geometric picture (acute angles)

This is the proof that shows *where the four terms come from*. It assumes $A$, $B$ and $A+B$ are all acute; Derivation 2 removes that restriction.

### Step 1 — set up the construction
Draw a ray from the origin $O$ at angle $A$ above the $x$-axis. Stack a second angle $B$ on top of it, and take the point $P$ on this outer ray with $OP = 1$, so $P$ sits at angle $A+B$:

```
  y
  ^
  |          P
  |          |                    OP = 1        (at angle A + B)
  |          |                    OQ = cos B    (at angle A)
  |          |                    QP = sin B    (perpendicular to OQ)
  |     T----Q
  |     |    |
  |     |    |
  O-----S----R--------> x
```

- $Q$ is the **foot of the perpendicular** dropped from $P$ onto the inner ray $OQ$.
- $R$ is the foot of the perpendicular from $Q$ onto the $x$-axis.
- $S$ is the foot of the perpendicular from $P$ onto the $x$-axis.
- $T$ is where the **horizontal** line through $Q$ meets the **vertical** line $PS$.

Because $OP = 1$, the coordinates of $P$ are exactly the numbers we want:
$$
P = \big(\cos(A+B),\ \sin(A+B)\big)
\qquad\Longrightarrow\qquad
\cos(A+B) = OS, \quad \sin(A+B) = PS
$$

### Step 2 — measure the right-angled triangle $OQP$
Angle $OQP = 90°$ by construction, the angle at $O$ is $B$, and the hypotenuse is $OP = 1$:
$$
OQ = OP\cos B = \cos B,
\qquad
QP = OP\sin B = \sin B
$$

### Step 3 — measure the right-angled triangle $OQR$
Angle $ORQ = 90°$, the angle at $O$ is $A$, and the hypotenuse is $OQ = \cos B$ from Step 2:
$$
OR = OQ\cos A = \cos A\cos B,
\qquad
QR = OQ\sin A = \sin A\cos B
$$

### Step 4 — find the angle in triangle $QTP$
$QP$ is perpendicular to $OQ$, and $OQ$ makes an angle $A$ with the horizontal. Rotating a line by $90°$ rotates the angle it makes with the horizontal by $90°$ too, so $QP$ makes an angle $90° - A$ with the horizontal line $QT$:
$$
\angle TQP = 90° - A
\qquad\Longrightarrow\qquad
\angle QPT = 180° - 90° - (90° - A) = A
$$

### Step 5 — measure the right-angled triangle $QTP$
Angle $QTP = 90°$ (horizontal meets vertical), the angle at $P$ is $A$ from Step 4, and the hypotenuse is $QP = \sin B$ from Step 2:
$$
PT = QP\cos A = \cos A\sin B,
\qquad
QT = QP\sin A = \sin A\sin B
$$

### Step 6 — read off $\sin(A+B)$
$QRST$ is a rectangle, so its opposite sides are equal: $TS = QR$. The vertical segment $PS$ splits at $T$:
$$
\sin(A+B) \;=\; PS \;=\; PT + TS \;=\; PT + QR
$$
Substituting Step 5 and Step 3:
$$
\boxed{\ \sin(A+B) = \cos A\sin B + \sin A\cos B\ }
$$

### Step 7 — read off $\cos(A+B)$
Along the $x$-axis, $OS = OR - SR$, and $SR = QT$ (opposite sides of the same rectangle):
$$
\cos(A+B) \;=\; OS \;=\; OR - SR \;=\; OR - QT
$$
Substituting Step 3 and Step 5:
$$
\boxed{\ \cos(A+B) = \cos A\cos B - \sin A\sin B\ }
$$
The minus sign is now not a rule to memorise: it is there because $P$ lies **left** of $Q$, so the horizontal step $QT$ is *subtracted*.

---

## Derivation 2 — rotation matrices (valid for all angles)

The picture above only works while every angle is acute. This version has no such limit, and it is three lines.

### Step 1 — the rotation matrix
Rotating the plane anticlockwise by $\theta$ is the linear map
$$
R(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}
$$

### Step 2 — rotating twice is rotating once by the total
Rotating by $B$ and then by $A$ is the same as rotating by $A+B$, and composing linear maps is matrix multiplication:
$$
R(A)\,R(B) = R(A+B)
$$

### Step 3 — multiply the left-hand side out
$$
\begin{pmatrix} \cos A & -\sin A \\ \sin A & \cos A \end{pmatrix}
\begin{pmatrix} \cos B & -\sin B \\ \sin B & \cos B \end{pmatrix}
=
\begin{pmatrix}
\cos A\cos B - \sin A\sin B & -(\sin A\cos B + \cos A\sin B) \\
\sin A\cos B + \cos A\sin B & \cos A\cos B - \sin A\sin B
\end{pmatrix}
$$

### Step 4 — compare with the right-hand side
$$
R(A+B) = \begin{pmatrix} \cos(A+B) & -\sin(A+B) \\ \sin(A+B) & \cos(A+B) \end{pmatrix}
$$
Matching entry by entry gives both formulae at once, for **any** $A$ and $B$:
$$
\cos(A+B) = \cos A\cos B - \sin A\sin B,
\qquad
\sin(A+B) = \sin A\cos B + \cos A\sin B
$$

> [!tip] Same argument in one line with Euler
> $e^{i\theta} = \cos\theta + i\sin\theta$, so
> $$e^{i(A+B)} = e^{iA}e^{iB} = (\cos A + i\sin A)(\cos B + i\sin B)$$
> Expanding the bracket and using $i^2 = -1$:
> $$= \underbrace{(\cos A\cos B - \sin A\sin B)}_{\text{real}} + \, i\underbrace{(\sin A\cos B + \cos A\sin B)}_{\text{imaginary}}$$
> Comparing real and imaginary parts with $e^{i(A+B)} = \cos(A+B) + i\sin(A+B)$ gives the pair again. This is why the same $\cos\omega t$/$\sin\omega t$ pairing shows up in [[Complex conjugate roots]].

---

## Getting the difference formulae

You do not prove these separately — you substitute $-B$ for $B$ and use the symmetry of the functions:
$$
\cos(-B) = \cos B \quad(\text{even}), \qquad \sin(-B) = -\sin B \quad(\text{odd})
$$

### Step 1 — sine
$$
\sin(A-B) = \sin\big(A + (-B)\big) = \sin A\cos(-B) + \cos A\sin(-B)
$$
### Step 2 — apply the symmetries
$$
= \sin A\cos B + \cos A(-\sin B)
$$
$$
\boxed{\ \sin(A-B) = \sin A\cos B - \cos A\sin B\ }
$$

### Step 3 — cosine
$$
\cos(A-B) = \cos\big(A + (-B)\big) = \cos A\cos(-B) - \sin A\sin(-B)
$$
### Step 4 — apply the symmetries
$$
= \cos A\cos B - \sin A(-\sin B)
$$
$$
\boxed{\ \cos(A-B) = \cos A\cos B + \sin A\sin B\ }
$$

---

## Deriving the tangent formula

### Step 1 — write $\tan$ as a ratio
$$
\tan(A+B) = \frac{\sin(A+B)}{\cos(A+B)} = \frac{\sin A\cos B + \cos A\sin B}{\cos A\cos B - \sin A\sin B}
$$

### Step 2 — divide top and bottom by $\cos A\cos B$
This is the whole trick: it is legal whenever $\cos A\cos B \neq 0$, and it converts every term into a $\tan$.
$$
= \frac{\dfrac{\sin A\cos B}{\cos A\cos B} + \dfrac{\cos A\sin B}{\cos A\cos B}}
       {\dfrac{\cos A\cos B}{\cos A\cos B} - \dfrac{\sin A\sin B}{\cos A\cos B}}
$$

### Step 3 — cancel
Each fraction collapses because $\frac{\sin\theta}{\cos\theta} = \tan\theta$:
$$
= \frac{\tan A + \tan B}{1 - \tan A\tan B}
$$
$$
\boxed{\ \tan(A+B) = \frac{\tan A + \tan B}{1 - \tan A\tan B}\ }
$$

### Step 4 — the difference version
Replace $B$ with $-B$ and use $\tan(-B) = -\tan B$ (odd over even, so odd):
$$
\boxed{\ \tan(A-B) = \frac{\tan A - \tan B}{1 + \tan A\tan B}\ }
$$

---

## Immediate consequences

### Double angle formulae — set $B = A$
$$
\sin 2A = \sin A\cos A + \cos A\sin A = \boxed{2\sin A\cos A}
$$
$$
\cos 2A = \cos A\cos A - \sin A\sin A = \boxed{\cos^2 A - \sin^2 A}
$$
Using $\sin^2 A + \cos^2 A = 1$ to eliminate one term at a time gives the two other faces of the same identity:
$$
\cos 2A = 2\cos^2 A - 1 = 1 - 2\sin^2 A
$$
$$
\tan 2A = \frac{2\tan A}{1 - \tan^2 A}
$$

### Power-reduction / half-angle — rearrange $\cos 2A$
Starting from $\cos 2A = 1 - 2\sin^2 A$ and solving for $\sin^2 A$:
$$
2\sin^2 A = 1 - \cos 2A \implies \boxed{\sin^2 A = \frac{1 - \cos 2A}{2}}
$$
and from $\cos 2A = 2\cos^2 A - 1$:
$$
\boxed{\cos^2 A = \frac{1 + \cos 2A}{2}}
$$
These two are how you integrate $\sin^2$ and $\cos^2$ — see Worked example 3 and [[trig|trig integration]].

### Product-to-sum — add or subtract the pairs
Add the two sine formulae; the $\cos A\sin B$ terms cancel:
$$
\sin(A+B) + \sin(A-B) = 2\sin A\cos B
\implies
\sin A\cos B = \tfrac12\big[\sin(A+B) + \sin(A-B)\big]
$$
Add the two cosine formulae; the $\sin A\sin B$ terms cancel:
$$
\cos A\cos B = \tfrac12\big[\cos(A-B) + \cos(A+B)\big]
$$
Subtract them instead; the $\cos A\cos B$ terms cancel:
$$
\sin A\sin B = \tfrac12\big[\cos(A-B) - \cos(A+B)\big]
$$

### The $R$-formula (combining two waves)
Any sum of a sine and a cosine at the *same* frequency is a single shifted sine wave:
$$
a\sin\theta + b\cos\theta = R\sin(\theta + \alpha)
$$
**Why:** expand the right side with the compound angle formula:
$$
R\sin(\theta+\alpha) = R\sin\theta\cos\alpha + R\cos\theta\sin\alpha = (R\cos\alpha)\sin\theta + (R\sin\alpha)\cos\theta
$$
Matching the $\sin\theta$ and $\cos\theta$ coefficients:
$$
a = R\cos\alpha, \qquad b = R\sin\alpha
$$
Squaring and adding kills $\alpha$; dividing kills $R$:
$$
\boxed{\ R = \sqrt{a^2+b^2}, \qquad \tan\alpha = \frac{b}{a}\ }
$$

---

## Worked example 1 — exact value of $\sin 75°$
$75°$ is not a standard angle, but it splits into two that are: $75 = 45 + 30$.

**Step 1 — split the angle:**
$$
\sin 75° = \sin(45° + 30°)
$$
**Step 2 — apply the formula:**
$$
= \sin 45°\cos 30° + \cos 45°\sin 30°
$$
**Step 3 — substitute the exact values** $\sin45° = \cos45° = \frac{\sqrt2}{2}$, $\cos30° = \frac{\sqrt3}{2}$, $\sin30° = \frac12$:
$$
= \frac{\sqrt2}{2}\cdot\frac{\sqrt3}{2} + \frac{\sqrt2}{2}\cdot\frac{1}{2}
= \frac{\sqrt6}{4} + \frac{\sqrt2}{4}
$$
**Step 4 — combine:**
$$
\boxed{\ \sin 75° = \frac{\sqrt6 + \sqrt2}{4}\ }
$$
> [!check] Verify
> $\frac{2.449 + 1.414}{4} = 0.9659$, and $\sin 75° = 0.9659$ ✓

## Worked example 2 — exact value of $\tan 75°$
**Step 1 — split:** $\tan 75° = \tan(45° + 30°)$

**Step 2 — apply the formula** with $\tan 45° = 1$ and $\tan 30° = \frac{1}{\sqrt3}$:
$$
\tan 75° = \frac{1 + \frac{1}{\sqrt3}}{1 - 1\cdot\frac{1}{\sqrt3}}
$$
**Step 3 — clear the inner fractions** by multiplying top and bottom by $\sqrt3$:
$$
= \frac{\sqrt3 + 1}{\sqrt3 - 1}
$$
**Step 4 — rationalise** by multiplying top and bottom by the conjugate $(\sqrt3+1)$:
$$
= \frac{(\sqrt3+1)^2}{(\sqrt3-1)(\sqrt3+1)} = \frac{3 + 2\sqrt3 + 1}{3 - 1} = \frac{4 + 2\sqrt3}{2}
$$
$$
\boxed{\ \tan 75° = 2 + \sqrt3\ }
$$
> [!check] Verify
> $2 + 1.732 = 3.732$, and $\tan 75° = 3.732$ ✓

## Worked example 3 — $\displaystyle\int \sin^2 x\,dx$
A compound angle identity is what makes this integrable at all: there is no rule for integrating $\sin^2 x$ *as written*, so you flatten the power first.

**Step 1 — replace the square** using the power-reduction identity above:
$$
\int \sin^2 x\,dx = \int \frac{1 - \cos 2x}{2}\,dx
$$
**Step 2 — split the integral:**
$$
= \frac{1}{2}\int 1\,dx - \frac{1}{2}\int \cos 2x\,dx
$$
**Step 3 — integrate each piece** (the extra $\frac12$ on the second comes from the chain rule, as in [[trig|trig integration]]):
$$
= \frac{x}{2} - \frac{1}{2}\cdot\frac{1}{2}\sin 2x + C
$$
$$
\boxed{\ \int \sin^2 x\,dx = \frac{x}{2} - \frac{\sin 2x}{4} + C\ }
$$
> [!check] Verify by differentiating back
> $\frac{d}{dx}\!\left(\frac{x}{2} - \frac{\sin 2x}{4}\right) = \frac12 - \frac{2\cos 2x}{4} = \frac{1 - \cos 2x}{2} = \sin^2 x$ ✓

## Worked example 4 — combining two waves: $3\sin\theta + 4\cos\theta$
**Step 1 — choose the target form:**
$$
3\sin\theta + 4\cos\theta = R\sin(\theta + \alpha)
$$
**Step 2 — expand the target** with the compound angle formula:
$$
R\sin(\theta+\alpha) = (R\cos\alpha)\sin\theta + (R\sin\alpha)\cos\theta
$$
**Step 3 — match coefficients** of $\sin\theta$ and $\cos\theta$:
$$
R\cos\alpha = 3, \qquad R\sin\alpha = 4
$$
**Step 4 — square and add** (using $\sin^2 + \cos^2 = 1$):
$$
R^2(\cos^2\alpha + \sin^2\alpha) = 3^2 + 4^2 = 25 \implies R = 5
$$
**Step 5 — divide** to get the phase:
$$
\tan\alpha = \frac{4}{3} \implies \alpha = \arctan\tfrac43 \approx 53.13° = 0.927\ \text{rad}
$$
$$
\boxed{\ 3\sin\theta + 4\cos\theta = 5\sin(\theta + 0.927)\ }
$$
This is the standard move for reading off the **amplitude** and **phase shift** of an oscillation, and it is why the solution of an oscillator can be written either as $C_1\cos\omega t + C_2\sin\omega t$ or as a single shifted wave — see [[Complex conjugate roots]].

> [!check] Verify at $\theta = 0$
> Left: $3(0) + 4(1) = 4$. Right: $5\sin(0.927) = 5(0.8) = 4$ ✓

## Worked example 5 — the oscillator solution $x(t) = C\cos\omega t + D\sin\omega t = A\sin(\omega t + \theta)$

Solving $\ddot{x} + \omega^2 x = 0$ hands you the answer as **two** separate waves with arbitrary constants $C$ and $D$. Physically there is only **one** oscillation, with an amplitude and a starting offset. The compound angle formula is exactly the bridge between the two descriptions.

**Step 1 — expand the target form.** Apply $\sin(A+B)$ with "$A$" $= \omega t$ and "$B$" $= \theta$:
$$
A\sin(\omega t + \theta) = A\big[\sin\omega t\cos\theta + \cos\omega t\sin\theta\big]
$$
**Step 2 — group it as a combination of $\cos\omega t$ and $\sin\omega t$:**
$$
A\sin(\omega t + \theta) = \underbrace{(A\sin\theta)}_{\text{const}}\cos\omega t + \underbrace{(A\cos\theta)}_{\text{const}}\sin\omega t
$$
This is the key observation: $\theta$ is a *constant*, so $A\sin\theta$ and $A\cos\theta$ are just numbers. The shifted wave is already a mixture of $\cos\omega t$ and $\sin\omega t$ **at the same frequency** — which is why the two forms can be equal at all.

**Step 3 — match coefficients.** We need
$$
C\cos\omega t + D\sin\omega t = (A\sin\theta)\cos\omega t + (A\cos\theta)\sin\omega t
\qquad\text{for all } t
$$
$\cos\omega t$ and $\sin\omega t$ are linearly independent (neither is a multiple of the other), so the only way this holds for *every* $t$ is for the coefficients to agree separately:
$$
\boxed{C = A\sin\theta}, \qquad \boxed{D = A\cos\theta}
$$
> [!warning] Note which one is which
> $C$ is the **cosine** coefficient and it lands on $\sin\theta$; $D$ is the **sine** coefficient and it lands on $\cos\theta$. The letters cross over. Deriving it each time beats memorising it.

**Step 4 — solve for $A$ by squaring and adding.** The cross terms never appear, and $\sin^2\theta + \cos^2\theta = 1$ eliminates $\theta$ entirely:
$$
C^2 + D^2 = A^2\sin^2\theta + A^2\cos^2\theta = A^2(\sin^2\theta + \cos^2\theta) = A^2
$$
$$
\boxed{\ A = \sqrt{C^2 + D^2}\ } \qquad (\text{take } A > 0)
$$

**Step 5 — solve for $\theta$ by dividing.** Dividing the two matched equations eliminates $A$:
$$
\frac{C}{D} = \frac{A\sin\theta}{A\cos\theta} = \tan\theta
\qquad\Longrightarrow\qquad
\boxed{\ \tan\theta = \frac{C}{D}\ }
$$
**Step 6 — fix the quadrant.** $\arctan$ only returns angles in $(-90°, 90°)$, so it cannot tell $\theta$ from $\theta + 180°$. Go back to Step 3 for the signs:
$$
\sin\theta = \frac{C}{A}, \qquad \cos\theta = \frac{D}{A}
$$
$\theta$ sits in the quadrant where those two signs agree — i.e. $\theta = \operatorname{atan2}(C, D)$. If $C > 0$ and $D > 0$, plain $\arctan(C/D)$ is already correct.

**The result:**
$$
\boxed{\ C\cos\omega t + D\sin\omega t = \sqrt{C^2+D^2}\,\sin(\omega t + \theta), \qquad \tan\theta = \frac{C}{D}\ }
$$

### Reading the result off
| Quantity | Value | Meaning |
|---|---|---|
| Amplitude | $A = \sqrt{C^2+D^2}$ | peak displacement either side of zero |
| Angular frequency | $\omega$ | unchanged — mixing same-frequency waves cannot create a new frequency |
| Period | $T = \dfrac{2\pi}{\omega}$ | unchanged by $C$ and $D$ |
| Phase | $\theta$ | the wave is shifted **left** in time by $\dfrac{\theta}{\omega}$ |
| Value at $t=0$ | $x(0) = A\sin\theta = C$ | consistent with putting $t=0$ into the original |

So $C$ and $D$ trade off *only* amplitude and phase. Both forms have two free constants ($C, D$ versus $A, \theta$), as they must for a second-order equation.

### Numbers — $x(t) = \cos 2t + \sqrt3\,\sin 2t$
Here $C = 1$, $D = \sqrt3$, $\omega = 2$.

**Amplitude:**
$$
A = \sqrt{1^2 + (\sqrt3)^2} = \sqrt{1 + 3} = 2
$$
**Phase:**
$$
\tan\theta = \frac{C}{D} = \frac{1}{\sqrt3} \implies \theta = \frac{\pi}{6}\ (30°)
$$
Both $C > 0$ and $D > 0$, so $\theta$ is in the first quadrant and no adjustment is needed.
$$
\boxed{\ \cos 2t + \sqrt3\,\sin 2t = 2\sin\!\left(2t + \frac{\pi}{6}\right)\ }
$$
> [!check] Verify at two times
> **$t = 0$:** left $= 1 + 0 = 1$; right $= 2\sin\frac{\pi}{6} = 2 \cdot \tfrac12 = 1$ ✓
> **$t = \frac{\pi}{4}$ (so $2t = \frac{\pi}{2}$):** left $= 0 + \sqrt3 = \sqrt3$; right $= 2\sin\frac{2\pi}{3} = 2\cdot\frac{\sqrt3}{2} = \sqrt3$ ✓

> [!check] Verify by matching initial conditions
> Differentiating both forms and setting $t = 0$:
> $$\dot{x} = -C\omega\sin\omega t + D\omega\cos\omega t \implies \dot{x}(0) = D\omega$$
> $$\dot{x} = A\omega\cos(\omega t + \theta) \implies \dot{x}(0) = A\omega\cos\theta = D\omega \;\checkmark$$
> The two forms agree on both the starting position and the starting velocity, so they are the same solution.

### The cosine variant
The same algebra against $A\cos(\omega t - \phi)$ instead — expand with $\cos(A-B) = \cos A\cos B + \sin A\sin B$:
$$
A\cos(\omega t - \phi) = (A\cos\phi)\cos\omega t + (A\sin\phi)\sin\omega t
$$
$$
\implies C = A\cos\phi, \quad D = A\sin\phi
\implies
\boxed{\ A = \sqrt{C^2+D^2}, \qquad \tan\phi = \frac{D}{C}\ }
$$
Same amplitude, and the ratio is simply flipped relative to the sine form. The two phases are related by $\theta = \frac{\pi}{2} - \phi$, since $\sin(u + \theta) = \cos(u - \phi)$ when the arguments differ by a quarter-turn. Either form is fine — just be consistent about which one you are using.

> [!warning] Radians
> If $\omega$ is in rad/s then $\omega t$ is in radians, so $\theta$ must be in radians too before you add them.

---

## Common mistakes

> [!warning] Watch for these
> - Writing $\sin(A+B) = \sin A + \sin B$. It is false for almost every pair of angles.
> - Getting the cosine sign backwards: $\cos(A+B)$ takes a **minus**, $\cos(A-B)$ takes a **plus**. It is the opposite of what the bracket looks like.
> - Mixing the terms in cosine. Cosine pairs **like with like** ($\cos\cos$, $\sin\sin$); only sine mixes them.
> - Using the $\tan$ formula when $\tan A\tan B = 1$ — the denominator is zero because $A + B$ is an odd multiple of $90°$, and $\tan(A+B)$ genuinely does not exist there. Fall back to $\sin$ and $\cos$.
> - Forgetting that $\cos 2A$ has **three** equivalent forms; picking the wrong one usually costs an extra line of algebra rather than an error.
> - In the $R$-formula and $A\sin(\omega t + \theta)$ work: taking the phase ratio the wrong way up, or trusting $\arctan$ to give the right quadrant. Always go back to the two matched equations for the signs.

---

## Connections
- Part of [[Trigonometry]].
- Reduces to the double-angle and half-angle identities, which are the standard way into $\int\sin^2$ and $\int\cos^2$ — see [[trig|trig integration]].
- Worked example 5 is the step used in [[dampened harmonic oscillator]] to write $C\cos\omega t + D\sin\omega t$ as $A\sin(\omega t + \theta)$.
- The complex-exponential proof relies on the index laws in [[exp-algebria-rules]], and is the same manoeuvre as in [[linear homogenious equations]].
- Converts the two-constant solution $C\cos\omega t + D\sin\omega t$ of an oscillator into a single amplitude-and-phase wave (Worked example 5) — the form that appears in [[Complex conjugate roots]] and [[Second Order differential Equations]].
- The product-to-sum forms turn products of trig functions into sums, which is what makes them integrable without [[by-parts|integration by parts]].
- The factors of $a$ on $\sin(ax)$ throughout come from [[chain-rule|the chain rule]].
- The Euler-formula proof is the same algebra as the complex exponential solutions in [[Complex conjugate roots]].
- The shortest proof of all: dot two unit vectors at angles $A$ and $B$ — $\hat{a}\cdot\hat{b}$ is $\cos A\cos B + \sin A\sin B$ by components and $\cos(A-B)$ by definition. See [[Scalar Product]].
- The rotation-matrix proof is the composition of two rotations in [[Euclidean space]].
