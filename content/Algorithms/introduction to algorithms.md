Algorithms generally have inputs and outputs. A sorting algorithm can be denoted by the following:
$$
\text{input: } a_1, \ldots, a_n \quad \text{and} \quad \text{output: } a_1 \leq a_2 \ldots \leq a_n
$$
Correctness for an algorithm is a procedure to ensure that all inputs have all correct outputs. The algorithm needs to be well specified so we can prove it. For instance a search engine result is not well defined. How do we know the suggestion is good? But Google will use a series of algorithms to get there.

Orders of description go with the following:

1. picture
2. pseudocode
3. code

An example of an algorithm is to find the shortest path for a load of dots. Here the input is $(x_1, y_1), (x_2, y_2) \ldots (x_n, y_n)$ and the output is the order.

Nearest neighbour tour is a simple approach which is:

```
pick p = p_0 and i = 0
while there are still unvisited points
    i = i + 1
    let p_i be the closest unvisited point to p_(i-1)
    visit p_i
return to p_0 from p_i
```

This algorithm will find a path but the final result might have to make a large leap at the end and the sum might be longer overall even without the long leap as it's the very definition of short sighted. The best way to show an algorithm is wrong is to just show one example where it doesn't work.

A correct example is exhaustive search where we try every possible combination with the following:

```
d = ∞
for each of the n! permutations Π_i of the n points
    if (cost(Π_i) <= d) then
        d = cost(Π_i) and P_min = Π_i
return P_min
```

- $d = \infty$ is a variable holding the best (shortest) tour length found so far. It starts at infinity so that the very first candidate is going to beat it.
- $\Pi_i$ is used here as a name for a permutation, with `i` is the ordering of the points.
- $\text{cost}(\Pi_i)$ is the cost function which is the total length of the tour and it takes the form below:

$$
\text{cost}(\Pi) = \sum_{k=1}^{n-1} \text{dist}(\Pi(k), \Pi(k+1))
$$

In this case:
$$
\text{dist}(p, q) = \sqrt{(p_x - q_x)^{2} + (p_y - q_y)^{2}}
$$

- $n!$ is just to calculate the number of permutations with `n` degrees of freedom.

For this introduction the professor states that there isn't an efficient solution.

## Connections
- The $\text{dist}(p, q)$ used by the cost function is the straight-line metric of [[Euclidean space]] — the tour length is a sum of Pythagorean distances, and the points are the position vectors of [[Basic Operations]].
- Nearest neighbour is greedy: it takes the best local step and never reconsiders, which is why one counterexample is enough to sink it. Exhaustive search is correct but pays $n!$ for it.
- Proving a cost function's closed form, and the induction that turns "no counterexample found" into an actual proof: [[basic recursion for sum loop]].
- A worked example of the picture → pseudocode → code ordering on a real algorithm, ending in a closed form: [[Cylinder]].
- Part of [[Home]].

