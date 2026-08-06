First order differential equations can be solved by direct integration. This just takes the following differential equation:
$$
\frac{dy}{dx} = f(x)
$$
We then have the following solution:
$$
\int \frac{dy}{dx} dx = \int f(x) dx
$$
And the deltas behave the same way as everything else in algebra. We can cancel out the `dx` to give the following:
$$
\int dy = \int f(x) dx => y = \int f(x) dx = F(x) + C
$$
If we have the following complication:
$$
\frac{dy}{dx} = g(x)g(y)
$$
We merely separate the variables with the following:
$$
\int \frac{1}{dy} dy = \int g(x) dx
$$
And then we work on separate integrations.

## Example
We have the following differential equation:
$$
\frac{dy}{dx} = \frac{y - 1}{x}
$$
With this we have the following equation definitions:
$$
\begin{aligned}
g(x) &= \frac{1}{x} \\[6pt]
g(y) &= y - 1
\end{aligned}
$$
We can then separate the variables giving us the following:
$$
\int \frac{1}{y - 1} dy = \int \frac{1}{x} dx
$$
This gives us the following:
$$
\begin{aligned}
ln|y - 1| = ln|x| + C => y - 1 = e^{ln|x| + c} \\[6pt]
y - 1 = e^{c}e^{ln|x|} => y - 1 = Ax
\end{aligned}
$$
Therefore the solution is:
$$
y = 1 + Ax
$$
