lines can take the following form:
$$
\{\,(x, y) \mid ax + by = c,\ (a, b) \neq (0, 0)\,\}
$$
in Euclidian space. The standard form of a line is defined by the following:
$$
ax + by = c
$$
We can have the following rearrangement of the line into "slope intercept" form:
$$
ax + by = c => by = -ax + c => y = \frac{-a}{b}x + \frac{a}{b} => y = mx + b 
$$
Where the end result `m` is the slope and `b` is the intercept on the `y` axis. This form makes lines easy to compare, and ready to graph. For instance, we already know the intercept by collapsing `x` to zero. However, the intercept form can collapse. If we have a vertical line the slope can be infinite.

We can also derive the point slope form with the following:
$$
m = \frac{y - y_{1}}{x - x_{1}} => y - y_{1} = m(x - x_{1})
$$
The point slope form is usually the most common way in which a geometric information arrives. It is also a natural starting point for a line given the two points. It is also a good starting bridge. Point slope is usually the first thing you write then you expand to slope intercept or general form.

We can go one step further. If we are handed two points $(x_{1}, y_{1})$ and $(x_{2}, y_{2})$ rather than a point and a slope, we do not know `m` up front. But the slope is just the rise over the run between those two points:
$$
m = \frac{y_{2} - y_{1}}{x_{2} - x_{1}}
$$
Substituting that slope back into the point slope form gives us the **two point form**:
$$
y - y_{1} = \frac{y_{2} - y_{1}}{x_{2} - x_{1}}(x - x_{1})
$$
This says the same thing as point slope, but with the slope written directly in terms of the two known points, so no separate slope calculation is needed.

The two point form is generally used when the geometry gives you *two locations* rather than a direction. This is the most common real world situation: you know two positions (two GPS coordinates, two data samples, two corners of a shape) and want the line through them. It is the natural way to reconstruct a line from raw sampled data, to interpolate between two known values, or to define an edge between two vertices. Once written, it expands into slope intercept form (for graphing and comparison) or general form $ax + by = c$ (the symmetric, division free form that also handles vertical lines).

Note that this form still breaks down for a vertical line: if $x_{1} = x_{2}$ the denominator $x_{2} - x_{1}$ becomes zero and the slope is undefined. In that case the line is simply $x = x_{1}$, which is why the general form $ax + by = c$ is preferred when you need to cover every possible line without exceptions.