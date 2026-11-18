---
tags:
  - algorithms
  - data-structures
related: "[[introduction to algorithms]]"
created: 2026-10-31
---

# Ring Buffer

The ring buffer takes the following form:

```rust
#[derive(Debug)]
pub struct RingBuffer<T> {
    buffer: Vec<Option<T>>,
    ptr: usize,
    len: usize,
}

impl<T> RingBuffer<T> {
    pub fn new(capacity: usize) -> Self {
        assert!(capacity > 0, "capacity must be greater than zero");

        Self {
            buffer: std::iter::repeat_with(|| None)
                .take(capacity)
                .collect(),
            ptr: 0,
            len: 0,
        }
    }

    /// Advances the pointer, wrapping around the buffer.
    fn increment_pointer(&mut self) {
        self.ptr = (self.ptr + 1) % self.buffer.len();
    }

    /// Moves the pointer backwards, wrapping around.
    fn decrement_pointer(&mut self) {
        if self.ptr == 0 {
            self.ptr = self.buffer.len() - 1;
        } else {
            self.ptr -= 1;
        }
    }

    /// Inserts a value, overwriting the oldest when full.
    pub fn push(&mut self, value: T) {
        self.buffer[self.ptr] = Some(value);

        self.increment_pointer();

        if self.len < self.buffer.len() {
            self.len += 1;
        }
    }

    /// Returns the oldest value without removing it.
    pub fn oldest(&self) -> Option<&T> {
        if self.len == 0 {
            return None;
        }

        let index = if self.len == self.buffer.len() {
            self.ptr
        } else {
            0
        };

        self.buffer[index].as_ref()
    }

    /// Returns the newest value without removing it.
    pub fn newest(&self) -> Option<&T> {
        if self.len == 0 {
            return None;
        }

        let index = if self.ptr == 0 {
            self.buffer.len() - 1
        } else {
            self.ptr - 1
        };

        self.buffer[index].as_ref()
    }

    pub fn len(&self) -> usize {
        self.len
    }

    pub fn capacity(&self) -> usize {
        self.buffer.len()
    }
}
```

## Connections

- The direct sibling is [[Linked Lists]], which does the same job with the opposite trade: here the data never moves and the *pointer* wraps with `% buffer.len()`, there the data stays put and the links are repointed. A rotation in a ring is one index assignment; in a chain it is a re-stitch of three `next` fields.
- `(self.ptr + 1) % self.buffer.len()` is a division, and when the capacity is a power of two it is the mask `(p + 1) & (n - 1)` of [[Bit Shifting operations]] — the $2^k - 1$ low mask of [[Binary Numbers]], which is why production ring buffers are nearly always sized to a power of two.
- A sliding window of the last few minutes of measurements is exactly this structure, which is how the percentile windows of [[Describing Performance]] are kept: fixed capacity, the oldest sample overwritten by the newest, nothing allocated per sample.
- Overwriting rather than growing is a bounded-memory decision taken up front, which is the backpressure question of [[Trade-offs in Data Systems Architecture]] in one data structure — drop, block or buffer, with `push` here choosing to drop the oldest.
- Part of [[Home]].
