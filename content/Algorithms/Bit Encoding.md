# Packing two fields into one integer

Worked example: the object `Id` in the GPU driver, `gpu-driver/src/cad/meshes/buffers/id.rs`. Every mesh in the scene needs a name that is one number, because that number gets written into the id texture by the fragment shader and read back a pixel at a time to answer "what did the user just click on". A pair `(kind, index)` would be two numbers. The encoding turns the pair into a single `u32` and back again with no lookup table and no loss.

The whole approach is the quotient–remainder split of [[Binary Numbers]] given names: pick a column to cut the integer at, put one field above it and one below.

---

## 1. The layout

A `u32` has 32 columns. The cut is at column 29:

```
 bit 31           29 28                                    0
  ┌───┬───┬───┬───┬───────────────────────────────────────┐
  │ k │ k │ k │   index (29 bits)                         │
  └───┴───┴───┴───────────────────────────────────────────┘
    3 bits kind            0 .. 2^29 - 1
```

In code that is two constants and a derived one:

```rust
const KIND_SHIFT: u32 = 29;
const INDEX_MASK: u32 = (1 << Self::KIND_SHIFT) - 1; // 0x1FFF_FFFF
pub const MAX_INDEX: u32 = Self::INDEX_MASK;
```

`INDEX_MASK` is `(1 << 29) - 1`, which is $2^{29} - 1$: twenty-nine 1s, the all-ones pattern from [[Binary Numbers]]. It is simultaneously the mask that keeps the index columns and the largest index that fits, which is why `MAX_INDEX` is just an alias for it.

| Field | Bits | Range | Holds |
|---|---|---|---|
| kind | 3 | $0 \ldots 7$ | which allocation buffer: `None`, `Cylinder`, `Sphere`, `Cone`, `Plane`, `Mesh`, `Group`, one spare |
| index | 29 | $0 \ldots 536\,870\,911$ | the slot inside that buffer |

---

## 2. Encoding

$$\text{raw} = (k \ll 29) \;\lor\; i$$

```rust
pub const fn new(kind: Kind, index: u32) -> Id {
    debug_assert!(index <= Self::MAX_INDEX);
    Id(((kind as u32) << Self::KIND_SHIFT) | index)
}
```

Take `Kind::Sphere` (the enum is `#[repr(u8)]` with `Sphere = 2`) at index 5.

**Shift the kind up.** $2 \ll 29$ moves the pattern `10` from columns 0 and 1 to columns 29 and 30:

$$\texttt{0100 0000 0000 0000 0000 0000 0000 0000} = \texttt{0x4000\_0000}$$

**OR the index in.** $5 = \texttt{101}$ sits in columns 0 to 2, which the shifted kind has left as zeros:

$$\texttt{0100 0000 0000 0000 0000 0000 0000 0101} = \texttt{0x4000\_0005} = 1\,073\,741\,829$$

> [!note] OR here is addition
> The two operands have no column in common — the kind occupies 31..29, the index 28..0 — so nothing carries and $\lor$ and $+$ give the same answer. Arithmetically the raw value is
>
> $$\text{raw} = k \cdot 2^{29} + i, \qquad 0 \le i < 2^{29}$$
>
> which is base-$2^{29}$ positional notation: $k$ is the digit, $i$ is the remainder. OR is preferred because it says "these fields are disjoint" and because it stays correct if a third field is added later. The same disjointness argument, and what goes wrong when it fails, is the OR-versus-addition warning in [[Bitset Auth]].

---

## 3. Decoding

$$k = \text{raw} \gg 29 \qquad i = \text{raw} \land \texttt{0x1FFF\_FFFF}$$

```rust
pub const fn kind(self) -> Kind { Kind::from_bits(self.0 >> Self::KIND_SHIFT) }
pub const fn index(self) -> u32 { self.0 & Self::INDEX_MASK }
```

On `0x4000_0005`:

**Kind.** Shifting right by 29 drops the 29 index columns off the bottom and leaves the top three:

$$\texttt{0100 0000 \ldots 0101} \gg 29 = \texttt{010} = 2 \;\Rightarrow\; \texttt{Kind::Sphere}$$

**Index.** AND with the mask clears the top three columns and keeps the rest:

$$
\begin{aligned}
&\texttt{0100 0000 0000 0000 0000 0000 0000 0101} \\
\land\;&\texttt{0001 1111 1111 1111 1111 1111 1111 1111} \\
=\;&\texttt{0000 0000 0000 0000 0000 0000 0000 0101} = 5
\end{aligned}
$$

The pair goes in and the same pair comes out. Formally the map

$$(k, i) \mapsto k \cdot 2^{29} + i$$

is a bijection from $\{0..7\} \times \{0..2^{29}-1\}$ onto $\{0 .. 2^{32}-1\}$, because the division algorithm gives exactly one quotient and one remainder for every raw value. That is what the `round_trips` test asserts for every kind at indices $0$, $1$ and `MAX_INDEX`.

---

## 4. The reserved values

Two slots in the encoding are deliberately spent rather than used:

```rust
pub const NONE: Id = Id(0);
pub fn is_none(self) -> bool { self.raw() == 0 }
```

- **`Kind::None = 0`.** With kind 0 and index 0 the whole word is zero, so the null id is the all-zero pattern — which is also what a cleared texture and a zeroed buffer contain. A miss in the picking pass therefore decodes to "nothing" without any extra signalling.
- **Kind 7 is spare.** Three bits give eight values for seven named kinds.

`from_bits` is a total function over all eight:

```rust
const fn from_bits(bits: u32) -> Kind {
    match bits {
        1 => Kind::Cylinder, 2 => Kind::Sphere, 3 => Kind::Cone,
        4 => Kind::Plane,    5 => Kind::Mesh,   6 => Kind::Group,
        _ => Kind::None, // 0, and the spare 7
    }
}
```

This matters because the input is a `u32` that has been out to the GPU and back. Any 32-bit pattern decodes to *some* kind, so a corrupt or stale pixel is read as `None` instead of panicking or indexing a buffer that does not exist.

---

## 5. Why the split is where it is

The choice is a straight trade between how many kinds and how many objects of each:

| Kind bits | Kinds | Index bits | Max index |
|---|---|---|---|
| 2 | 4 | 30 | $1\,073\,741\,823$ |
| **3** | **8** | **29** | **$536\,870\,911$** |
| 4 | 16 | 28 | $268\,435\,455$ |
| 8 | 256 | 24 | $16\,777\,215$ |

Seven kinds is a design fact about the driver, so 3 bits is the smallest field that fits with room for one more. Everything left over goes to the index, and half a billion slots per kind is not a limit a CAD scene will reach. Note the *total* is always $2^{32}$ — bits are conserved, so a field only grows by taking columns from another.

---

## 6. Increment without touching the kind

```rust
pub const fn next(self) -> Option<Id> {
    if self.index() == Self::MAX_INDEX {
        None
    } else {
        Some(Id(self.0 + 1)) // safe: can't carry into the kind bits
    }
}
```

The interesting part is that `+ 1` is applied to the **whole word**, not to the index field. Ordinary addition carries from column to column, so incrementing would corrupt the kind if the index were all ones — $\texttt{0x3FFF\_FFFF} + 1 = \texttt{0x4000\_0000}$ turns a `Cylinder` into a `Sphere`. The guard makes that unreachable: below `MAX_INDEX` the index has at least one zero column left, so the carry chain stops inside the index field and the top three bits are untouched.

`Table::new` leans on exactly this — the allocator's high-water mark starts at a raw id and counts up:

```rust
Self { live: Vec::new(), high_water: Id::new(kind, 0).raw(), free: Vec::new() }
```

so the kind rides along for free on every allocation, and `Id::from_raw(id).index()` is what turns the id back into a `Vec` subscript.

> [!warning] `debug_assert!` is not a check in release
> `new` asserts `index <= MAX_INDEX`, but `debug_assert!` compiles out in release builds. An oversized index then simply carries into the kind field and both values come back wrong:
>
> $$\text{new}(\texttt{Sphere}, 2^{29}) = (2 \ll 29) \lor \texttt{0x2000\_0000} = \texttt{0x6000\_0000}$$
>
> which decodes as kind $3$ (`Cone`), index $0$. Silent, and a corrupt pick rather than a crash. The real protection is that indices come from the allocator, which cannot exceed `MAX_INDEX` because `next` refuses to.

---

## 7. Why a `u32` at all

```rust
#[repr(transparent)]
#[derive(Clone, Copy, Debug, PartialEq, Eq, Hash)]
pub struct Id(u32);
```

- `#[repr(transparent)]` guarantees `Id` has the identical layout to the `u32` inside it, so it can be written straight into a GPU buffer or texture with no conversion step. `raw()` and `from_raw()` are the two ends of that crossing — out to the shader, back from the picker.
- `Copy` and four bytes wide means the methods take `self` by value. As the source notes, a reference would be an 8-byte pointer to 4 bytes of data: bigger than the thing it points at.
- `Eq` and `Hash` come free because the identity *is* an integer, so ids can key a map with no custom logic.

The encoding also has to survive a round trip through a texture, which is where the sibling encoding in `kernel/colour.rs` comes in. `PickColor` packs an id into an RGBA8 pixel, 8 bits per channel:

$$R = (\text{id} + 1) \land \texttt{0xFF}, \quad G = ((\text{id} + 1) \gg 8) \land \texttt{0xFF}, \quad B = ((\text{id} + 1) \gg 16) \land \texttt{0xFF}$$

Same two operations, three fields instead of two, 24 usable bits so about 16.7 million ids. The $+1$ is the same reservation trick as `Id::NONE`: it keeps $(0,0,0)$ meaning "background, no hit" rather than "object 0".

---

## Symbols

| Symbol | Meaning |
|---|---|
| $k$ | the kind, $0 \ldots 7$, 3 bits |
| $i$ | the index, $0 \ldots 2^{29}-1$, 29 bits |
| raw | the packed `u32`, $k \cdot 2^{29} + i$ |
| `KIND_SHIFT` | 29, the column the fields are cut at |
| `INDEX_MASK` | $2^{29} - 1$ = `0x1FFF_FFFF`, 29 ones |
| `MAX_INDEX` | the same number read as a limit |
| $\ll$, $\gg$ | shift up (multiply by $2^k$), shift down (divide) |
| $\land$, $\lor$ | bitwise AND (keep columns), OR (merge disjoint fields) |
| `NONE` | the all-zero id, kind `None` and index 0 |

---

## Connections
- The prerequisite: [[Binary Numbers]]. $\text{raw} = k \cdot 2^{29} + i$ is positional notation in base $2^{29}$, the mask is the $2^k - 1$ identity, and decode is the division algorithm — none of this is specific to bits, it is place value with a cut in it.
- [[Bitset Auth]] is the other half of the pair: same operators, opposite reading. There a bit means *membership*, so masks overlap and $\lor$ is union; here fields are *disjoint*, so $\lor$ is concatenation and overlap is the bug. Both notes turn a structured value into one integer so the hot path is arithmetic.
- The kinds being packed are the mesh families of [[Cylinder]] and [[Sphere]]; an id names one allocation slot in the buffers those notes fill, and [[Mesh Writer Cursors]] is the open question about typing those writes.
- Picking is the inverse of rendering: the scene pass writes ids into a texture, the pick reads one pixel and decodes it. The projection that got the geometry onto that pixel is [[CAD Geometry]], and the ray/point it corresponds to lives in [[Euclidean space]].
- Reserving the all-zero pattern for "nothing" is the same convention as the empty OR being 0 in [[Bitset Auth]] and the empty sum in [[basic recursion for sum loop]]: the identity element doubles as the safe default.
- Correctness as a specified procedure, and one counterexample being enough to sink it — [[introduction to algorithms]]. The `round_trips` test is the specification written down; the release-mode `debug_assert!` gap is the counterexample it does not cover.
- Index laws behind the shifts, $2^a 2^b = 2^{a+b}$: [[exp-algebria-rules]].
- Part of [[Home]].
