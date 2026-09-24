# Binary numbers and place value

Binary is base 2 written in the same positional system as ordinary decimal: each column has a value, and the digit in that column says how many of it you have. The only difference is that the column values are powers of $2$ instead of powers of $10$, and the digit is restricted to $0$ or $1$.

This is the note that [[Bit Encoding]] and [[Bitset Auth]] assume: once a number *is* its columns, packing two numbers into one integer and pulling them apart again is arithmetic rather than a trick.

---

## The value of a number from its bits

Write the bits as $b_{n-1} b_{n-2} \ldots b_1 b_0$, where $b_k \in \{0, 1\}$ and $n$ is the width. The value is

$$v = \sum_{k=0}^{n-1} b_k \, 2^{k}$$

Bit $k$ is worth $2^k$, so every bit is worth exactly twice the one to its right. $b_0$ is the **least significant bit** (LSB), the rightmost one; $b_{n-1}$ is the **most significant bit** (MSB).

Decimal does the same thing with $10^k$:

$$405 = 4 \times 10^2 + 0 \times 10^1 + 5 \times 10^0$$

The sum has at most one term per column because a bit is 0 or 1, so in binary the formula degenerates into something easier than decimal: **add up the column values wherever there is a 1, ignore the 0s.**

| $k$ | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| $2^k$ | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

---

## Worked example 1: `1011`

| Bit | $b_3 = 1$ | $b_2 = 0$ | $b_1 = 1$ | $b_0 = 1$ |
|---|---|---|---|---|
| Column value | $2^3 = 8$ | $2^2 = 4$ | $2^1 = 2$ | $2^0 = 1$ |
| Contributes | 8 | — | 2 | 1 |

$$v = 1 \times 2^3 + 0 \times 2^2 + 1 \times 2^1 + 1 \times 2^0 = 8 + 2 + 1 = 11$$

---

## Worked example 2: `101101`

| Bit | $b_5 = 1$ | $b_4 = 0$ | $b_3 = 1$ | $b_2 = 1$ | $b_1 = 0$ | $b_0 = 1$ |
|---|---|---|---|---|---|---|
| Column value | 32 | 16 | 8 | 4 | 2 | 1 |
| Contributes | 32 | — | 8 | 4 | — | 1 |

$$v = 32 + 8 + 4 + 1 = 45$$

> [!note] Parity for free
> $b_0$ is the only odd column value, since every other column is a multiple of 2. So a number is odd exactly when its last bit is 1, which is why `x & 1` is the standard odd test.

---

## Reading it left to right instead

Summing columns means knowing the width before you start. The other way round needs no lookahead: start at $v = 0$ and, for each bit from the left,

$$v \leftarrow 2v + b$$

On `101101`:

| Bit read | 1 | 0 | 1 | 1 | 0 | 1 |
|---|---|---|---|---|---|---|
| $v$ after | 1 | 2 | 5 | 11 | 22 | 45 |

Same answer, 45. This is Horner's rule: doubling shunts everything already accumulated one column left, then the new bit lands in the ones column. It is the recurrence form of the sum above, and it is how a parser reads digits off a stream.

---

## Decimal to binary

**Repeated division by 2.** Divide, keep the remainder, repeat on the quotient. The remainders are the bits, least significant first, so read them **bottom to top**.

| Step | Quotient | Remainder |
|---|---|---|
| $45 \div 2$ | 22 | **1** |
| $22 \div 2$ | 11 | **0** |
| $11 \div 2$ | 5 | **1** |
| $5 \div 2$ | 2 | **1** |
| $2 \div 2$ | 1 | **0** |
| $1 \div 2$ | 0 | **1** |

Read upwards: `101101` $= 45$. Each division strips the ones column off and shifts the rest down, which is exactly what the $2v + b$ recurrence does in reverse.

**Or subtract the largest power of two that fits.** $45 - 32 = 13$, $13 - 8 = 5$, $5 - 4 = 1$, $1 - 1 = 0$. Bits 5, 3, 2 and 0 are set, everything else is 0: `101101`. This works, and the representation is unique, because each power of two is larger than the sum of all the smaller ones ($2^k > 2^k - 1$), so taking the biggest can never be a mistake.

---

## Width, leading zeros and range

A fixed-width type is $n$ columns whether or not you use them, so $45$ in a `u8` is `0010 1101`: the leading zeros are real columns holding 0. With $n$ bits there are $2^n$ distinct patterns, giving unsigned values

$$0 \le v \le 2^n - 1$$

| Type | $n$ | Max value |
|---|---|---|
| `u8` | 8 | $255$ |
| `u16` | 16 | $65\,535$ |
| `u32` | 32 | $4\,294\,967\,295$ |
| `u64` | 64 | $\approx 1.8 \times 10^{19}$ |

The maximum is $2^n - 1$ rather than $2^n$ because one pattern is spent on zero. All-ones is the largest:

$$\underbrace{1 1 \ldots 1}_{n} = \sum_{k=0}^{n-1} 2^k = 2^n - 1$$

which is the geometric series, and the identity that makes masks work further down. Going the other way, the number of bits needed to hold a value $N \ge 1$ is

$$n = \lfloor \log_2 N \rfloor + 1$$

so 45 needs $\lfloor 5.49 \rfloor + 1 = 6$ bits, matching `101101`.

---

## Hex, because 32 bits is unreadable

Four bits hold $0$ to $15$, which is one hexadecimal digit, so hex is binary with the bits grouped in fours — a pure re-spelling, no arithmetic involved.

| Hex | Bits | Hex | Bits |
|---|---|---|---|
| `0` | `0000` | `8` | `1000` |
| `1` | `0001` | `9` | `1001` |
| `2` | `0010` | `A` | `1010` |
| `4` | `0100` | `D` | `1101` |
| `7` | `0111` | `F` | `1111` |

$$\texttt{0xD6} = \texttt{1101}\ \texttt{0110} = 128 + 64 + 16 + 4 + 2 = 214$$

A `u32` is eight hex digits. `0x1FFF_FFFF` is `0001` followed by seven `1111` groups, i.e. 29 ones, which is the index mask of [[Bit Encoding]] read straight off the page.

---

## Shifting is multiplying by powers of two

Moving every bit one column left doubles each column value, so

$$x \ll k = x \cdot 2^{k} \qquad x \gg k = \left\lfloor \frac{x}{2^{k}} \right\rfloor$$

$$45 \ll 2 = \texttt{101101} \to \texttt{10110100} = 180 = 45 \times 4$$

$$45 \gg 2 = \texttt{101101} \to \texttt{1011} = 11 = \left\lfloor \tfrac{45}{4} \right\rfloor$$

The right shift floors because the bits pushed off the end are the remainder, and they are discarded. Shifts compose the way the index laws say they should — $(x \ll a) \ll b = x \cdot 2^a \cdot 2^b = x \ll (a + b)$ is just $2^a 2^b = 2^{a+b}$ — see [[exp-algebria-rules]].

> [!warning] Left shifts fall off the end
> $x \ll k$ is $x \cdot 2^k$ only while the result still fits in $n$ bits. Bits shifted past the MSB are gone, so in a `u32`, $1 \ll 32$ is not $2^{32}$; it is undefined behaviour in C and a panic or wrap in Rust depending on the build. Every shift carries an implicit "and it fits".

---

## Masks keep the columns you want

AND-ing with a pattern of $k$ ones keeps the low $k$ columns and clears everything above them. Since those columns are the ones worth less than $2^k$, the result is the remainder:

$$x \land (2^{k} - 1) = x \bmod 2^{k}$$

$$45 \land 7 = \texttt{101101} \land \texttt{000111} = \texttt{000101} = 5 = 45 \bmod 8$$

Pair that with a shift and a number splits cleanly in two at column $k$:

$$x = \underbrace{(x \gg k)}_{q} \cdot 2^{k} + \underbrace{(x \land (2^{k} - 1))}_{r}, \qquad 0 \le r < 2^{k}$$

which is the division algorithm: $q$ and $r$ are the unique quotient and remainder. Uniqueness is the whole point — it means a single integer can carry two independent numbers with no ambiguity about which is which, provided the lower one is known to stay under $2^k$. That is the entire idea behind [[Bit Encoding]].

---

## Signed numbers in one paragraph

Everything above is unsigned, which is what the GPU ids of [[Bit Encoding]] use. Signed integers keep the same columns but read the top one as negative: in two's complement an `i8` is $-b_7 2^7 + \sum_{k=0}^{6} b_k 2^k$, so `1111 1111` is $-128 + 127 = -1$. Addition and subtraction are unchanged, which is why hardware prefers it, but right shift and "is this bit set" questions have to know which interpretation is in play.

---

## Symbols

| Symbol | Meaning |
|---|---|
| $b_k$ | the bit in column $k$, $0$ or $1$ |
| $2^k$ | the value of column $k$ |
| LSB / MSB | least / most significant bit — column $0$ / column $n-1$ |
| $n$ | width in bits |
| $\ll$, $\gg$ | left shift, right shift |
| $\land$, $\lor$ | bitwise AND, OR |
| $2^k - 1$ | a mask of $k$ ones |
| $\bmod$ | remainder after division |
| `0x` | hexadecimal literal, one digit per 4 bits |
| `0b` | binary literal |

---

## Connections
- The direct sequel: [[Bit Encoding]] splits a `u32` at column 29 and stores two numbers in it, using nothing but the shift, mask and quotient–remainder facts derived here.
- [[Bitset Auth]] uses the same columns for a different job — a bit is set membership rather than a place value, so $\lor$ is union instead of addition. Atoms $a(r) = 2^{\iota(r)}$ are single columns, which is exactly why OR and $+$ agree on them and diverge once masks overlap.
- $\sum_{k=0}^{n-1} 2^k = 2^n - 1$ is a finite sum with a closed form, proved the same way as $1 + 2 + \cdots + n$ in [[basic recursion for sum loop]]; $v \leftarrow 2v + b$ is a recurrence of the kind that note is about.
- Index laws $2^a 2^b = 2^{a+b}$ and $\lfloor \log_2 N \rfloor + 1$ come from [[exp-algebria-rules]], part of [[Foundations]]. Shifting is those laws executed in hardware.
- Reading a number as a vector of digits against a vector of column values, $v = \sum b_k 2^k$, is a dot product of the bits with the place values — [[Scalar Product]].
- Positional notation is a choice of base the way a coordinate system is a choice of basis; base 2 and base 16 describe the same integer in different columns, as [[Basic Operations]] writes the same vector in different components.
- Correctness by well-specified procedure, in the sense of [[introduction to algorithms]]: both conversions above terminate and both are provably unique, which is what lets code round-trip through them.
- Part of [[Home]].
