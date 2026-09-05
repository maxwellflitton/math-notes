A vector can be denoted by:
$$
\mathbf{a} = a_x\mathbf{i} + a_y\mathbf{j} + a_z\mathbf{k} \quad \text{(component form)}
$$
And the magnitude can be done by the following:
$$
a = |\mathbf{a}| = \sqrt{a_x^{2} + a_y^{2} + a_z^{2}}
$$
For an example of these forms we can explore the following equation:
$$
\mathbf{a} = \mathbf{i} + \mathbf{j} + \mathbf{k} \quad \text{and} \quad \mathbf{b} = 2\mathbf{i} - 3\mathbf{j} - \mathbf{k} \quad \text{and} \quad \mathbf{c} = 3\mathbf{i} + \mathbf{k}
$$
We can express the following equations in coordinate form:
$$
\mathbf{d} = 2\mathbf{a} - 3\mathbf{b} \quad \text{and} \quad \mathbf{e} = \mathbf{a} - 2\mathbf{b} + 4\mathbf{c}
$$
Which can be done with the following:
$$
\begin{aligned}
\mathbf{d} &= 2(\mathbf{i} + \mathbf{j} + \mathbf{k}) - 3(2\mathbf{i} - 3\mathbf{j} - \mathbf{k}) = -4\mathbf{i} + 11\mathbf{j} + 5\mathbf{k} \\[6pt]
\mathbf{e} &= (\mathbf{i} + \mathbf{j} + \mathbf{k}) - 2(2\mathbf{i} - 3\mathbf{j} - \mathbf{k}) + 4(3\mathbf{i} + \mathbf{k}) = 9\mathbf{i} + 7\mathbf{j} + 7\mathbf{k}
\end{aligned}
$$
We can calculate the magnitude of these equations with the following:
$$
|\mathbf{d}| = \sqrt{(-4)^{2} + 11^{2} + 5^{2}} = \sqrt{162} = 9\sqrt{2}
$$
$$
|\mathbf{e}| = \sqrt{9^{2} + 7^{2} + 7^{2}} = \sqrt{179}
$$
We can evaluate $|\mathbf{a}|$ and write down the unit vector with the following:
$$
|\mathbf{a}| = \sqrt{1^{2} + 1^{2} + 1^{2}} = \sqrt{3} \implies \hat{\mathbf{a}} = \frac{\mathbf{a}}{|\mathbf{a}|} = \frac{1}{\sqrt{3}}(\mathbf{i} + \mathbf{j} + \mathbf{k})
$$
The unit vector takes the following form:
$$
\hat{\mathbf{a}} = \frac{\mathbf{a}}{|\mathbf{a}|} = \left(\frac{a_x}{a}, \frac{a_y}{a}, \frac{a_z}{a}\right) = (\cos\theta_x, \cos\theta_y, \cos\theta_z)
$$
We can prove that:
$$
\hat{\mathbf{a}} = \left(\frac{a_x}{a}, \frac{a_y}{a}, \frac{a_z}{a}\right)
$$
Has a magnitude of one with the following:
$$
\hat{a} = \sqrt{\left(\frac{a_x}{a}\right)^{2} + \left(\frac{a_y}{a}\right)^{2} + \left(\frac{a_z}{a}\right)^{2}} = \frac{\sqrt{a_x^{2} + a_y^{2} + a_z^{2}}}{a} = \frac{|\mathbf{a}|}{a} = 1
$$
Scaling vectors have the following form:
$$
\lambda\mathbf{a} = (\lambda a_x, \lambda a_y, \lambda a_z) = \lambda a_x\mathbf{i} + \lambda a_y\mathbf{j} + \lambda a_z\mathbf{k}
$$
And addition has the following:
$$
\mathbf{a} + \mathbf{b} = (a_x + b_x)\mathbf{i} + (a_y + b_y)\mathbf{j} + (a_z + b_z)\mathbf{k}
$$
We can also have a straight line between two vectors $\mathbf{q}$ and $\mathbf{p}$, we have the following equation:
$$
\mathbf{r}(t) = \mathbf{p} + t(\mathbf{q} - \mathbf{p}) = (1 - t)\mathbf{p} + t\mathbf{q}
$$
The $\mathbf{q} - \mathbf{p}$ is just the displacement vector and then the `t` is purely the scalar to work out a point on that line.

When it comes to calculus of a vector each component can be done separately. For example lets look at the following equation:
$$
\mathbf{r}(t) = (3t^{2} - 2,\ t^{4},\ -t + 1)
$$
$$
\frac{d}{dt}\mathbf{r}(t) = \frac{d}{dt}(3t^{2} - 2,\ t^{4},\ -t + 1) = (6t,\ 4t^{3},\ -1)
$$

## Connections
- Part of [[Linear Algebra]]; these vectors live in [[Euclidean space]] — the magnitude $\sqrt{a_x^2 + a_y^2 + a_z^2}$ is just the Pythagorean distance from the origin.
- $\mathbf{q} - \mathbf{p}$ being a displacement rather than a position is the distinction drawn in [[Displacement vs Position Vectors]] — head minus tail.
- $\mathbf{r}(t) = (1 - t)\mathbf{p} + t\mathbf{q}$ is the parametric form of [[Lines]], and the degree-1 case of [[Bezier Curves]]. At $t = 0$ you are at $\mathbf{p}$, at $t = 1$ you are at $\mathbf{q}$.
- The unit vector $\hat{\mathbf{a}}$ strips out magnitude and keeps only direction, which is why its components are the direction cosines.
- Differentiating componentwise means every rule from [[chain-rule|differentiation]] applies one component at a time.
