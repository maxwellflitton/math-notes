# Sets and Set-Builder Notation

A **set** is just a collection of objects (called *elements* or *members*). Set notation is a compact language for describing *which* objects belong to a set, often without listing them all out.

## Set-builder notation

Most interesting sets are described with **set-builder notation**, which has the general shape:

$$
\{\ \text{element pattern} \ \mid \ \text{conditions the element must satisfy}\ \}
$$

Read the vertical bar `|` as **"such that"**. So the whole thing reads: *"the set of all [element pattern] such that [conditions]"*.

## Worked example

Take the set that describes a straight line in the plane:

$$
\{\,(x, y) \mid ax + by = c,\ (a, b) \neq (0, 0)\,\}
$$

Breaking it down piece by piece:

| Part                 | Meaning                                                                                                                                                                         |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `{ ... }`            | The curly braces denote a **set** — everything inside describes its members.                                                                                                    |
| `(x, y)`             | The **element pattern**: each member of the set is an ordered pair of coordinates. This tells us *what kind of thing* is in the set (points in the plane).                      |
| `\|`                 | **"such that"** — separates the element pattern (left) from the conditions (right).                                                                                             |
| `ax + by = c`        | A **condition (predicate)**: only pairs `(x, y)` that satisfy this equation are included. This is the equation of a line.                                                       |
| `(a, b) \neq (0, 0)` | A second **condition**, joined by the comma (read as "and"). It requires that `a` and `b` are not *both* zero — otherwise the equation `0 = c` wouldn't describe a line at all. |

In words: *"the set of all points `(x, y)` such that `ax + by = c`, where `a` and `b` are not both zero."* That set **is** the line.

## Common symbols

| Symbol                                                       | Meaning                                                              |
| ------------------------------------------------------------ | -------------------------------------------------------------------- |
| $\in$                                                        | "is an element of" — e.g. $3 \in \{1, 2, 3\}$                        |
| $\notin$                                                     | "is not an element of" — e.g. $5 \notin \{1, 2, 3\}$                 |
| $\mid$ or $:$                                                | "such that" (both are used interchangeably in set-builder notation)  |
| $\forall$                                                    | "for all"                                                            |
| $\exists$                                                    | "there exists"                                                       |
| $\subseteq$                                                  | "is a subset of" (every element of the left set is in the right set) |
| $\subset$                                                    | "is a proper subset of" (subset, but not equal)                      |
| $\cup$                                                       | union — elements in *either* set                                     |
| $\cap$                                                       | intersection — elements in *both* sets                               |
| $\setminus$                                                  | set difference — elements in the left set but not the right          |
| $\varnothing$                                                | the empty set (no elements)                                          |
| $\mathbb{N}, \mathbb{Z}, \mathbb{Q}, \mathbb{R}, \mathbb{C}$ | the natural numbers, integers, rationals, reals, complex numbers     |

## More examples of set notation

**Listing (roster) notation** — just enumerate the elements:

$$
\{1, 2, 3, 4, 5\}
$$

**With a pattern / ellipsis** — when the pattern is clear:

$$
\{2, 4, 6, 8, \dots\}
$$

**Even natural numbers** via a condition:

$$
\{\, n \in \mathbb{N} \mid n \text{ is even} \,\}
$$

**Using a rule to generate elements** — the element pattern can be an expression:

$$
\{\, 2n \mid n \in \mathbb{Z} \,\}
$$

This is the set of all even integers — every member has the form $2n$ for some integer $n$.

**A bounded interval of reals:**

$$
\{\, x \in \mathbb{R} \mid 0 \leq x < 1 \,\}
$$

which is the half-open interval usually written $[0, 1)$.

**The unit circle** in the plane:

$$
\{\, (x, y) \in \mathbb{R}^2 \mid x^2 + y^2 = 1 \,\}
$$

**Multiple conditions** (all must hold, joined by commas or "and"):

$$
\{\, n \in \mathbb{Z} \mid n > 0,\ n < 10,\ n \text{ is prime} \,\} = \{2, 3, 5, 7\}
$$

## The key idea

Every set-builder expression answers two questions:

1. **What do the elements look like?** (the pattern on the left of the bar)
2. **Which ones qualify?** (the conditions on the right)

Once you can read those two halves, any set notation — no matter how dense — becomes straightforward to unpack.
