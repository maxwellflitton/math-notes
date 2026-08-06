A linear differential equation takes the following form:
$$
\frac{dy}{dx} + g(x)y = h(x)
$$
In order for the equation to be linear the dependent variable `y` must be linear. We can look at how to solve linear differential equations starting with the integrating factor. First we have the following:
$$
\frac{dy}{dx} + Ay = h(x)
$$
First of all, we multiply both sides by an exponent to `ax` and we get the following:
$$
e^{Ax} \frac{dy}{dx} + Ae^{Ax}y = e^{Ax}h(x)
$$
We can then use the product rule to substitute out and simplify the left hand side of the equation. We remember that the product rule is:
$$
\frac{d}{dx}[f(x)g(x)] = f'(x)g(x) + f'(x)g(x)
$$
We then define the following definitions:
$$
f(x) = e^{Ax} \quad g(x) = y \\[6pt]
f'(x) = Ae^{Ax} \quad y'(x) = \frac{dy}{dx}
$$
We then plug the equations into the product rule and we get the following:
$$
f'(x)g(x) + f'(x)g(x) = e^{Ax} \frac{dy}{dx} + Ae^{Ax}y = \frac{d}{dx}[f(x)g(x)] = \frac{dy}{dx}e^{ax}y
$$
The left hand side of the equation then collapses to the following:
$$
\frac{dy}{dx}e^{Ax}y = e^{Ax}h(x)
$$
We then integrate both sides giving us the following notation:
$$
\int \frac{dy}{dx}e^{Ax}y dx = \int e^{Ax}h(x) dx
$$
Which then can be refined to:
$$
e^{Ax}y = \int e^{Ax}h(x) dx
$$
So `y(x)` takes the following form:
$$
y(x) = e^{-Ax} \int e^{Ax}h(x) dx
$$
`h(x)` can be anything, like the environment surrounding a cup of tea working out when the tea becomes room temperature. 