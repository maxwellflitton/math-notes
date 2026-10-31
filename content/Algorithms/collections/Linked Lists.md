---
tags: [algorithms, data-structures, linked-list, rust]
related: "[[introduction to algorithms]]"
created: 2026-10-28
---

# Linked Lists

Linked lists are essentially arrays where the elements are not contiguous in memory but linked via pointers. This makes traversal slower but operations like rotation and insertion quicker because you do not have to shift the location of the other elements to fit with the result of the operation. To demonstrate these advantages we can show the rotation of a list in Rust.

## Rotation of list

In this problem we need to take the head of the list, and a number to rotate the list from. So for instance, if we had the following:

```text
head = [1,2,3,4,5]
k = 2
```

We get the following output:

```text
[4,5,1,2,3]
```

We went two from the back and now `4` is the head. We can approach this the simple way first by constructing a contiguous array and then reconstructing the list. It's an easy mental model but we are using more memory for the buffer and we essentially have to do two sweeps but it does solve the problem. First we define the linked list with the following code:

```rust
#[derive(PartialEq, Eq, Clone, Debug)]
pub struct ListNode {
  pub val: i32,
  pub next: Option<Box<ListNode>>
}
```

If we have a variable called `head` that takes the shape of `Option<Box<ListNode>>` we construct the buffer with the code below:

```rust
let mut buffer = Vec::new();

let mut ptr = head.as_ref();

// how to iterate through a simple linked list
while let Some(node) = ptr {
    buffer.push(node.val);
    ptr = node.next.as_ref();
}
```

Here we can see that we construct the buffer, get the header to start with, and then iterate through the pointers of the list to construct the contiguous array. Note that we do not have the length therefore we cannot preallocate. In production it makes sense to have a wrapper struct to keep track of metadata such as the tail and the length. However, for this simplified example we can just jump through pointers. We must also note that we take the header reference using the `as_ref`. The `as_ref` function essentially converts `&Option<T> → Option<&T>`. This means we borrow the `T` inside rather than consuming the `Option`: ownership of neither moves, we just get a reference to look through. Let's look at the following example to show why this is important:

```rust
let name = Some("Maxwell".to_string());

if let Some(value) = name {
    println!("{value}");
}

// ERROR: name was partially moved
println!("{name:?}");
```

We can solve this error with the following:

```rust
let name = Some("Maxwell".to_string());

if let Some(value) = name.as_ref() {
    println!("{value}");
}

// Works!
println!("{name:?}");
```

What we are essentially doing with `as_ref` is moving the reference inside the container meaning we can use the APIs without transferring ownership. Instead of cloning the data, `as_ref` creates a lightweight `Option` containing a reference to the existing data.

Now that we have constructed our array, we can get all the primitives of constructing the new linked list with the following code:

```rust
// Calculate where the rotated list starts
let n = buffer.len();
let starting_point = (n - (k as usize % n)) % n;

// Construct the first node
let mut head = Box::new(ListNode {
    val: buffer[starting_point],
    next: None,
});
```

Here we notice that we use the modulo to calculate the starting point from `k` and construct the header of the new linked list. We then loop through the buffer and construct the linked list with the code below:

```rust
// Borrow the tail, don't take ownership
let mut current_node = &mut head;
let mut buffer_ptr = (starting_point + 1) % n;

while buffer_ptr != starting_point {
    current_node.next = Some(Box::new(ListNode {
        val: buffer[buffer_ptr],
        next: None,
    }));

    // Move the mutable reference to the new tail
    current_node = current_node.next.as_mut().unwrap();

    buffer_ptr = (buffer_ptr + 1) % n;
}
```

Here we note that the loop takes the following three steps

1. construct the next node from the buffer and attach this to the `next` of the current node
2. update the `current_node` pointer (mutable reference) to the next node which is the one we just created.
3. We then increase the buffer pointer by one and exploit the modulo so once we get to the end of the buffer we go back to the start.

We must notice that we kill the loop when we get back to the starting point with the `starting_point`.

We can be more optimal without the need for a second buffer. We can do this with the following steps:

1. calculate the full length of the linked list
2. normalise the pointers for the rotation
3. find the new tail and then head
4. stitch the new head into the linked list

First, we need to loop through the list to calculate the length with the following code:

```rust
let mut len = 0;
let mut ptr = head.as_ref();

while let Some(node) = ptr {
    len += 1;
    ptr = node.next.as_ref();
}
```

This is nothing new, but now with the pointer, we can normalise the pointers for finding the head with the code below:

```rust
// Normalize the rotation
let k = k as usize % len;

if k == 0 {
    return head;
}

// The new tail is at index len - k - 1
let steps = len - k - 1;
```

Here we use the modulo to account for the overflow. We also note that if the position ends up being `0` there is going to be no rotation because the new head is to be at `0` which is the current head. We then do a second pass to find the new tail:

```rust
// Second pass: find the new tail
let mut slow = head.as_mut().unwrap();

for _ in 0..steps {
    slow = slow.next.as_mut().unwrap();
}
```

As the new head is going to be the next step from the tail so we just take it making the `next` for the tail to be `None` with the following code:

```rust
let mut new_head = slow.next.take();
```

We then take a mutable reference of the head and loop through until we get to the old tail and attach that to the old head with the code below:

```rust
let mut tail = new_head.as_mut().unwrap();

while tail.next.is_some() {
    tail = tail.next.as_mut().unwrap();
}

// Connect old tail to original head
tail.next = head;
```

And here we have the rotated list.

> [!note] Two things the snippets assume
> Both approaches divide by the list length, so `k as usize % n` panics with _attempt to calculate the remainder with a divisor of zero_ on an empty list — the real signature wants an early `if head.is_none() { return head; }`. The in-place version also needs `mut head` in the signature, since `head.as_mut()` borrows it mutably before `tail.next = head` moves it. Checked by compiling both: they build as written under NLL and give `[4,5,1,2,3]` for `k = 2`, with `k = 7` wrapping to the same answer and `k = 5` a no-op.

---

## Connections

- Sits with [[introduction to algorithms]]. Both rotations are $O(n)$, so the interesting difference is not the order of growth but what else they spend: the buffer version makes two sweeps and allocates $n$ extra slots, the in-place version makes three cheap passes (length, walk to the new tail, walk to the old tail) and allocates nothing. Cost is a vector, not a single number, and "same big-O" is where the comparison starts rather than ends.
- The direct sibling is [[Ring Buffer]], which is the same rotation idea with the opposite trade: a contiguous `Vec` where nothing moves and the _pointer_ wraps with `% buffer.len()`, against a chain where the data never moves and the links are repointed. Rotation in a ring is one index assignment; here it is a re-stitch of three `next` fields.
- That `%` wrap is the arithmetic of [[Binary Numbers]], and when the capacity is a power of two it is the mask `(p + 1) & (n - 1)` of [[Bit Shifting operations]] — the $2^k - 1$ low mask, which is why ring buffers are so often sized to powers of two.
- Pointer chasing _is_ graph traversal with one neighbour: `while let Some(node) = ptr { ptr = node.next.as_ref() }` is the hook-two loop of [[Breadth First Search]] on a graph where every node has out-degree one, which is also why the `parent` chain that note walks back from the target is a linked list in all but name.
- The alternative to `Option<Box<ListNode>>` is the arena: store the nodes in one `Vec` and make `next` an index rather than a pointer, which is exactly the `Table` and packed slot id of [[Bit Encoding]]. You trade pointer-chasing cache misses for index arithmetic, get `Copy` handles that borrow-check trivially, and pay with manual free-list bookkeeping instead of `Box` doing it.
- Walking a list is naturally recursive — `len(node) = 1 + len(node.next)` with the empty list as the base case — so the loops here are the tail-recursive version written out, in the sense of [[basic recursion for sum loop]]. Rust gives no tail-call guarantee, which is the practical reason these are loops: a recursive `Drop` on a long list can blow the stack.
- `as_ref`, `as_mut` and `take` are the three moves the borrow checker forces on a linked structure, and they are why this data structure is the standard Rust teaching example: `take()` is the only way to sever a link without cloning, because it swaps a `None` in and hands you the owned tail.
- "Cost is a vector, not a single number" is the same argument [[Trade-offs in Data Systems Architecture]] makes about whole systems: once the order of growth matches, the choice is decided by allocations, cache behaviour and which operation you are willing to make expensive.
- Part of [[Home]].

## Open

- [ ] The $O(1)$ rotation: keep a tail pointer, join the list into a cycle, walk to the new tail and break it — one pass instead of three, and the reason production lists carry length and tail metadata.
- [ ] Doubly linked lists in Rust, and why they need `Rc<RefCell<T>>` or raw pointers and `unsafe` — the point where the borrow checker stops being free.
- [ ] Cycle detection with Floyd's tortoise and hare, reusing the `slow` pointer already here.
- [ ] A cache-locality benchmark against `Vec` for the same workload, to put numbers behind "traversal is slower".
