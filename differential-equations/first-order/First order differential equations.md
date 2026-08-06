First order differential equations can be solved by direct integration if they have the following form:
$$
\frac{dy}{dx} = f(x)
$$
We can solve these equations with two different approaches.
Separation of variables:
$$
\frac{dy}{dx} = g(x)h(y)
$$
Integration factor:
$$
\frac{dy}{dx} + g(x)y = h(x)
$$
If the first order differential equation doesn't fit neatly into the form of 
$$\frac{dy}{dx} = f(x)$$ 
We can show how a first order differential equation works by modelling a basic birthrate. First, we have the following equations:
$$
\begin{aligned}
\text{number of births} \approx b\,p(t)\,\delta t \\
\text{number of deaths}  \approx c\,p(t)\,\delta t \\
\end{aligned}
$$
Where:
$$\delta p \approx p(t + \delta t) - p(t)$$
Which expands to:
$$\delta p \approx b\,p(t)\,\delta t \approx (b - c)\,p(t)$$
If we divide by $\delta t$ we get the following:
$$
\frac{\delta p}{\delta t} \approx (b - c)\,p(t)
$$
This gets more accurate as [[lim-zero-example|delta trends to zero]] giving us the following:
$$
\frac{d p}{d t} = (b - c)\,p(t)
$$
$$
\text{if } r = b - c \implies \frac{d p}{d t} = r\,p(t)
$$
as `b` and `c` are currently constants. So the growth rate is `r`. Since `p` appears on the right-hand side, we solve this by separation of variables — moving all the `p` terms to one side and all the `t` terms to the other before integrating:
$$
\begin{aligned}
dp = r\,p\,dt \implies \frac{1}{p} dp = r\,dt \\[10pt]
\int \frac{1}{p}\,dp = \int r\,dt \\[10pt]
\ln |p| = rt + C
\end{aligned}
$$
We then exponentiate both sides to get the following:
$$
\begin{aligned}
e^{\ln |p|} = e^{rt + C} \implies |p| = e^{C}\,e^{rt} \\[10pt]
p = p_0\,e^{rt} \quad \text{where } p_0 = e^{C}
\end{aligned}
$$
Here $p_0$ is the initial population: setting $t = 0$ gives $p(0) = p_0\,e^{0} = p_0$. So the population grows (or decays) exponentially at rate $r = b - c$ and we have the final form of:
$$
p(t) = p_0\,e^{rt}
$$
This means that `r` can give us a expodential trajectory. If `r < 0` then there is a decay. `r > 0` gives us a growth. However, this is a simplistic model as resources will alter the tractory. Too high population will start to decline due to resource constraints, and a small population will have loads of free resources. We can describe the population as the logistic function with the following equation:
$$
\frac{d P}{d t} = k\,P\left(1 - \frac{P}{M}\right)
$$
This is solved in full in [[population-example|the logistic worked example]], giving the
sigmoid solution $P(t) = \dfrac{M\,P_0}{P_0 + (M - P_0)\,e^{-kt}}$.