# Encoding user roles and permissions with bitsets

Worked example: a hospital server. The goal is to turn a role hierarchy into plain integers so that an auth check at request time is a single bitwise AND, with no tree walking.

The flow is:

1. give every role a bit
2. record who is directly under whom
3. expand each role into "everything it counts as" and pack that into one integer
4. OR a user's roles together into one mask
5. check that mask against a requirement
6. do the same for doctor grades in a separate vector
7. combine the two into a permit decision

---

## 1. Role atoms

First, we encode the roles by giving each one a bit position, using an indexing function $\iota$ (iota):

| Role | $\iota$ |
|---|---|
| Patient | 0 |
| Nurse | 1 |
| Doctor | 2 |
| ScrubNurse | 3 |
| WardNurse | 4 |
| Surgeon | 5 |

$\iota$ is not an operator, it is just the name of a function, the same way you would write $f$. The set notation for it is:

$$\iota : \{\text{Patient}, \text{Nurse}, \text{Doctor}, \text{ScrubNurse}, \text{WardNurse}, \text{Surgeon}\} \to \{0, 1, 2, 3, 4, 5\}$$

$\iota$ must be **injective**: no two roles may share an index, or they would collide on the same bit.

We turn the position into a mask with:

$$a(r) = 2^{\iota(r)}$$

$$a(\text{Doctor}) = 2^{\iota(\text{Doctor})} = 2^2 = 4 = \texttt{0b000100}$$

$$a(\text{Surgeon}) = 2^5 = 32 = \texttt{0b100000}$$

$a(r)$ is the **atom** of the role: a mask with exactly one bit set. In code it is `1 << ι(r)`.

---

## 2. The tree as a relation

Now we want to encode the tree relationships between the roles.

$R$ is the set of six roles. $R \times R$ is the Cartesian product: every possible ordered pair (role, role), 36 in total. The tree is a **subset** of those pairs:

$$P \subseteq R \times R$$

where $(x, y) \in P$ means "$x$ is directly under $y$". Note that $R \times R$ is every *possible* pair, while $P$ holds only the pairs that are actually links in the tree:

$$P = \{(\text{ScrubNurse}, \text{Nurse}),\ (\text{WardNurse}, \text{Nurse}),\ (\text{Surgeon}, \text{Doctor})\}$$

$P$ stores **direct** links only (a "covering relation"). If CardiacSurgeon were added under Surgeon, $P$ would hold $(\text{CardiacSurgeon}, \text{Surgeon})$ but not $(\text{CardiacSurgeon}, \text{Doctor})$. That second fact is implied, and step 3 recovers it.

The same information as a matrix:

$$A_{ij} = \begin{cases} 1 & \text{if } (r_i, r_j) \in P \\ 0 & \text{otherwise} \end{cases}$$

Rows are the child, columns are the parent, and both are indexed from 0 using $\iota$. So:

- $A_{3,1} = 1$: ScrubNurse (3) is under Nurse (1)
- $A_{4,1} = 1$: WardNurse (4) is under Nurse (1)
- $A_{5,2} = 1$: Surgeon (5) is under Doctor (2)

$$A = \begin{pmatrix} 0&0&0&0&0&0 \\ 0&0&0&0&0&0 \\ 0&0&0&0&0&0 \\ 0&1&0&0&0&0 \\ 0&1&0&0&0&0 \\ 0&0&1&0&0&0 \end{pmatrix}$$

Rows 0 to 2 are empty because Patient, Nurse and Doctor have no parent. Strictly this is a forest with three roots, not a single tree.

> [!note] Two things worth noticing
> - **Each row is already a bitset.** Column $j$ is bit $j$, so ScrubNurse's row is $\texttt{000010} = a(\text{Nurse})$. The whole matrix is a `[u8; 6]` where `A[child]` is the parent's atom.
> - **$A$ is strictly lower triangular.** Every parent has a lower index than its children. That guarantees no cycles, and means processing roles in index order always handles a parent before its children.

---

## 3. Closure: encoding the tree into a function

We can now encode the tree into a computational function. Start with the ancestor set:

$$\uparrow r = \{\, s : r \le s \,\}$$

- $\uparrow r$ is just the name of the thing being defined, "the up-set of $r$"
- $\{ \dots \}$ is "the set of"
- $s$ is a placeholder ranging over all roles in $R$
- $:$ is "such that"
- $r \le s$ is the condition $s$ must satisfy to be in the set

Read aloud: "up-$r$ is the set of all roles $s$ such that $r$ is at or below $s$." It is a filter over $R$.

$\le$ here is not numeric comparison. It means "is at or under in the tree", and it is built from $P$ with two extra rules:

- **reflexive**: every role is $\le$ itself
- **transitive**: if $r \le p$ and $p \le s$ then $r \le s$, so chains compose

For example:

- $\text{ScrubNurse} \le \text{Nurse}$ is true (parent)
- $\text{ScrubNurse} \le \text{ScrubNurse}$ is true (itself)
- $\text{ScrubNurse} \le \text{Doctor}$ is false, they are in different trees

Some pairs are not comparable in either direction (Nurse and Doctor), which is why this is a *partial* order.

An example of the set:

$$\uparrow \text{ScrubNurse} = \{\text{ScrubNurse}, \text{Nurse}\}$$

Intuitively, $\uparrow r$ is **everything $r$ counts as**.

We encode this set as an integer with:

$$m(r) = \bigvee_{s \in \uparrow r} a(s)$$

$\bigvee$ is to OR what $\sum$ is to addition: for each $s$ in the set, take its atom and OR it into the result. We essentially overlay the bit masks:

$$m(\text{ScrubNurse}) = 2^3 \lor 2^1 = \texttt{001000} \lor \texttt{000010} = \texttt{001010}$$

$\uparrow r$ and $m(r)$ are the same object in two representations: $\uparrow r$ is the set of roles, $m(r)$ is that set written in binary, one bit per member.

All six:

| $r$ | $\uparrow r$ | $m(r)$ |
|---|---|---|
| Patient | {Patient} | `000001` |
| Nurse | {Nurse} | `000010` |
| Doctor | {Doctor} | `000100` |
| ScrubNurse | {ScrubNurse, Nurse} | `001010` |
| WardNurse | {WardNurse, Nurse} | `010010` |
| Surgeon | {Surgeon, Doctor} | `100100` |

Therefore, if ScrubNurse's mask is compared against either the Nurse atom or the ScrubNurse atom, a bit will hit and pass. This means **no tree traversal at runtime**. The property that guarantees it:

$$r \le s \iff m(s) \subseteq m(r)$$

Lower in the tree means *more* bits, because you accumulate everything above you. Once $m$ is built you can throw away $P$, $A$ and the tree and still answer any "is $r$ under $s$?" question from the integers alone.

> [!warning] OR, not addition
> For atoms the two give the same number, since distinct powers of two never carry. They diverge as soon as masks overlap:
>
> $$\texttt{001010} \lor \texttt{010010} = \texttt{011010}$$
>
> $$\texttt{001010} + \texttt{010010} = \texttt{011100}$$
>
> Addition carries the doubled Nurse bit into bit 2, which switches Nurse **off** and Doctor **on**: a privilege escalation bug. OR is idempotent ($x \lor x = x$), which matches set union.

> [!note] Computing $m$ in practice
> Formally it is a fixed point: start with $m_0(r) = a(r)$ and repeatedly OR in the parents' masks until nothing changes,
>
> $$m_{k+1}(r) = m_k(r) \lor \bigvee_{(r,p) \in P} m_k(p)$$
>
> or in matrix form $A^* = I \lor A \lor A^2 \lor \cdots$ with OR as addition and AND as multiplication, where row $r$ of $A^*$ is $m(r)$. Because $A$ is lower triangular, a single sweep in index order is enough (see the Rust sketch at the end). This runs once at startup, not per request.

---

## 4. The user's mask

We can then build the user's mask. A user holds a set of raw roles $S \subseteq R$ (exactly what is stored against them in the database), and their expanded identity is:

$$U = \bigvee_{r \in S} m(r)$$

An example, someone who is both a scrub nurse and a ward nurse:

$$U = m(\text{ScrubNurse}) \lor m(\text{WardNurse}) = \texttt{001010} \lor \texttt{010010} = \texttt{011010}$$

Bits 1, 3 and 4 are set: Nurse, ScrubNurse, WardNurse. They were never explicitly assigned Nurse, but they have it because both of their roles inherit it.

Note that $\texttt{011010}$ is **not** one of the six role masks. The tree cannot express "scrub nurse and ward nurse", but the bitset can because it is just a set. Multi-role users need no extra machinery.

| | Input | Computed | Size |
|---|---|---|---|
| $m(r)$ | one role | once for the whole system | 6 entries |
| $U$ | one user | per user, typically at login | one integer in the session |

---

## 5. The three checks

Now we can perform checks. $U$ is the user and $Q$ is a requirement mask.

$\land$ is bitwise AND: the output bit is 1 only where both inputs are 1. So $U \land Q$ answers "which of the required bits does this user actually have?" The three checks are three different questions about that result:

| Check | Formula | Question |
|---|---|---|
| any of $Q$ | $U \land Q \ne 0$ | did at least one bit hit? |
| all of $Q$ | $U \land Q = Q$ | did every bit hit? |
| none of $N$ | $U \land N = 0$ | did nothing hit? (deny-list) |

The all-of form is the subset test, as:

$$Q \subseteq U \iff Q \land \lnot U = 0$$

$\lnot U$ is every bit the user lacks, so $Q \land \lnot U$ is "required bits that are missing", and the check passes when there are none.

Examples with $U = \texttt{011010}$:

Any of ScrubNurse or Surgeon, $Q = \texttt{101000}$:

$$\texttt{011010} \land \texttt{101000} = \texttt{001000} \ne 0 \quad \checkmark$$

All of ScrubNurse and WardNurse, $Q = \texttt{011000}$:

$$\texttt{011010} \land \texttt{011000} = \texttt{011000} = Q \quad \checkmark$$

All of ScrubNurse and Surgeon, $Q = \texttt{101000}$:

$$\texttt{011010} \land \texttt{101000} = \texttt{001000} \ne Q \quad \times$$

None of Patient, $N = \texttt{000001}$:

$$\texttt{011010} \land \texttt{000001} = \texttt{000000} \quad \checkmark$$

> [!warning] Build $Q$ from atoms $a(r)$, not closed masks $m(r)$
> Suppose an endpoint requires ScrubNurse and you wrongly use $Q = m(\text{ScrubNurse}) = \texttt{001010}$ with the any-of check. A plain Nurse, $U = \texttt{000010}$, arrives:
>
> $$\texttt{000010} \land \texttt{001010} = \texttt{000010} \ne 0 \quad \checkmark \text{ (wrong)}$$
>
> The Nurse bit inside the closed mask matched. With the atom $Q = a(\text{ScrubNurse}) = \texttt{001000}$ the result is correctly 0.
>
> Rule: the inheritance expansion happens **once, on the user side** (steps 3 and 4). Requirements stay as plain atoms.

---

## 6. Doctor grades: a chain in a separate vector

We can then encode doctor grades in a different vector. The grades form a chain, F1 < F2 < Registrar < Consultant, and the natural question is "at least registrar". A grade implies every grade beneath it, so we use a **thermometer encoding**:

| Grade | $g$ |
|---|---|
| F1 | `0001` |
| F2 | `0011` |
| Registrar | `0111` |
| Consultant | `1111` |

Read bit $k$ as "is *at least* grade $k$". A Registrar is at least F1, at least F2 and at least Registrar, so bits 0 to 2 are on. It is the same closure as step 3 on a straight line: your mask is everything you count as.

$G_u$ is the user's **grade mask**. It is a second, independent field, not derived from the role bits in $U$. It is built the same way as $U$, from the set of grades $S_g$ assigned to that user:

$$G_u = \bigvee_{x \in S_g} g(x)$$

A nurse has no grades assigned, so $S_g = \emptyset$ and the OR runs over nothing. An empty OR is $0$, because 0 is the identity for OR ($x \lor 0 = x$), the same way an empty sum is 0. So $\texttt{0000}$ is not a code meaning "nurse", it is the absence of any grade.

The check for "at least registrar" is the all-of test again:

$$G_u \land g(\text{Reg}) = g(\text{Reg})$$

Consultant:

$$\texttt{1111} \land \texttt{0111} = \texttt{0111} \quad \checkmark$$

F2:

$$\texttt{0011} \land \texttt{0111} = \texttt{0011} \ne \texttt{0111} \quad \times$$

Nurse:

$$\texttt{0000} \land \texttt{0111} = \texttt{0000} \ne \texttt{0111} \quad \times$$

Anything ANDed with 0 is 0, so an empty mask can never satisfy a requirement. Access has to be granted explicitly, which is the safe default.

> [!note] Simplification
> Because of the "at least" reading, testing the single Registrar bit is enough: $G_u \land \texttt{0100} \ne 0$. If bit 2 is on, the thermometer guarantees bits 0 and 1 are too. This is the step 5 rule again: expand on the user side, keep the requirement as an atom.

> [!note] The maths does not enforce "only doctors have grades"
> $U$ and $G_u$ are independent, so nothing in the algebra stops a Consultant grade being assigned to a nurse's account. That is a data invariant, validated once when roles are edited:
>
> $$G_u \ne 0 \implies U \land a(\text{Doctor}) \ne 0$$

> [!note] Why not just store the grade as a number?
> For a pure chain you could, and compare with `>=`. The thermometer uses the same AND machinery as roles, and it survives the chain breaking. In the NHS a staff grade / SAS doctor is a parallel track to registrar, not strictly above it. An integer rank cannot express "neither is above the other", but a bitmask can: rerun the step 3 closure on the new shape and the checks do not change.

---

## 7. The permit decision

Each user's identity is the pair $(U, G_u)$. A permit decision combines a role check and a grade check:

$$\text{permit} = (U \land Q = Q) \;\land\; (G_u \land Q_g = Q_g)$$

The inner $\land$ symbols are bitwise AND on masks. The outer one is an ordinary logical AND between two true/false results.

"The theatre lead must be a surgeon who is at least a registrar" can be enforced with two bit masks:

$$Q = a(\text{Surgeon}) = \texttt{100000} \qquad Q_g = g(\text{Reg}) = \texttt{0111}$$

| User | $U$ | $G_u$ | Role check | Grade check | Permit |
|---|---|---|---|---|---|
| Consultant surgeon | `100100` | `1111` | pass | pass | yes |
| F2 surgeon | `100100` | `0011` | pass | fail | no |
| Registrar, non-surgeon | `000100` | `0111` | fail | pass | no |
| Scrub nurse | `001010` | `0000` | fail | fail | no |

> [!note] Packing into one word
> Put roles in bits 0 to 5 and grades in bits 6 to 9 of a `u16`. Then the user is one integer $W = U \lor (G_u \ll 6)$, the requirement is one integer, and the whole permit decision is a single all-of check.

---

## Rust sketch

```rust
#[repr(u8)]
#[derive(Clone, Copy)]
enum Role { Patient, Nurse, Doctor, ScrubNurse, WardNurse, Surgeon }

// Step 2: the relation P, one parent per role.
const PARENT: [Option<Role>; 6] = [
    None, None, None,
    Some(Role::Nurse), Some(Role::Nurse), Some(Role::Doctor),
];

// Step 1: a(r) = 2^ι(r)
const fn atom(r: Role) -> u8 { 1 << r as u8 }

// Step 3: closed masks m(r). A single sweep works because parents
// always have a lower index than their children.
fn closed_masks() -> [u8; 6] {
    let mut m = [0u8; 6];
    for r in 0..6 {
        m[r] = 1 << r;
        if let Some(p) = PARENT[r] { m[r] |= m[p as usize]; }
    }
    m
}

// Step 4: U = OR of m(r) over the user's raw roles. Empty slice gives 0.
fn user_mask(m: &[u8; 6], roles: &[Role]) -> u8 {
    roles.iter().fold(0, |acc, r| acc | m[*r as usize])
}

// Step 5: the three checks. Build q and n from atom(), not from m.
fn any_of(u: u8, q: u8) -> bool { u & q != 0 }
fn all_of(u: u8, q: u8) -> bool { u & q == q }
fn none_of(u: u8, n: u8) -> bool { u & n == 0 }
```

---

## Symbols

| Symbol | Meaning |
|---|---|
| $\iota(r)$ | bit position of role $r$ |
| $a(r)$ | atom: mask with only $r$'s bit set |
| $R$ | the set of all roles |
| $R \times R$ | every ordered pair of roles |
| $P$ | the pairs that are direct child to parent links |
| $A$ | $P$ as a Boolean matrix, rows = child, columns = parent |
| $r \le s$ | $r$ is $s$, or somewhere under $s$ |
| $\uparrow r$ | the set of everything $r$ counts as |
| $m(r)$ | $\uparrow r$ packed into an integer |
| $S$ | raw roles assigned to a user |
| $U$ | the user's expanded role mask |
| $Q$, $N$ | requirement mask, deny mask |
| $g(x)$ | thermometer mask for grade $x$ |
| $G_u$ | the user's grade mask |
| $\lor$, $\bigvee$ | bitwise OR, OR over a set (union) |
| $\land$ | bitwise AND (intersection) |
| $\lnot$ | bitwise NOT (complement) |
| $\subseteq$ | subset, tested as `x & y == x` |

---

## Connections
- Sits with [[introduction to algorithms]]: the problem is only solvable because "does this user count as a nurse?" is well specified by the closure $\uparrow r$. The OR-versus-addition warning is that note's method in action — one counterexample, $\texttt{001010} + \texttt{010010}$ switching Doctor on, is enough to rule addition out.
- The whole note is set theory written in binary — [[sets]]. $\uparrow r = \{s : r \le s\}$ is set-builder notation, $P \subseteq R \times R$ is a subset of a Cartesian product, and $\lor$, $\land$, $\lnot$ are $\cup$, $\cap$ and complement one bit at a time.
- $\bigvee$ is to OR what $\sum$ is to addition, and the empty OR being $0$ (the nurse with no grades) is the same convention as the empty sum in [[basic recursion for sum loop]]. The fixed point $m_{k+1}(r)$ is a recurrence, and the single index-order sweep is what lets it be computed in one pass.
- Atoms $a(r) = 2^{\iota(r)}$ are distinct powers of two, so their binary expansions share no bits — which is why OR and $+$ agree on atoms and only diverge once masks overlap. Index laws: [[exp-algebria-rules]].
- A bitset is a vector of 0s and 1s, and $U \land Q$ followed by a popcount is the dot product over $\{0, 1\}$. The none-of check $U \land N = 0$ is literally orthogonality — [[Scalar Product]].
- $A$ is a Boolean matrix, and $A^* = I \lor A \lor A^2 \lor \cdots$ is ordinary matrix powers with OR in place of $+$ and AND in place of $\times$; lower-triangular is what makes a single sweep enough. See [[Linear Algebra]].
- Pay once so the hot path is arithmetic on an integer: the closure here runs at startup exactly as the closed form in [[Cylinder]] replaces a loop, so the per-request check never walks the tree.
- Part of [[Home]].

