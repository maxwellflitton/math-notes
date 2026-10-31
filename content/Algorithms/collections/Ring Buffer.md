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