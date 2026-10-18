When it comes to bit operations, we are taking in a bit pattern which is what we are inspecting, and also a position for the inspection. If the index of the inspection is a `0` we return a `false`, and if the it is a `1` we return a `true`. We can see this in an example of the number `5` which we can represent with `101` giving us the following:

- `n=0`: `true`
- `n=1`: `false`
- `n=2`: `true`

The check starts from the right so `n=0` for `100` is `false`. We can define a get bit function with the following code:

```rust
fn get_bit(value: u32, n: u32) -> bool {
    if n >= 32 {
        panic!("overflow");
    }
    (value >> n) & 1 == 1
}
```

The `u32` is shifted to the right `n` spaces and then we perform an `&` operation against `1`. Because it's an `&` operation we know that all other bits will collpase to zero. If the result is `1` we can confirm that the end shifted bit is `1`. If the result is zero then we know that the shifted bit isn't there.

With the `get_bit` function in mind we can switch a bit on with the following function:

```rust
fn set_bit(value: u32, n: u32) -> u32 {
    if n >= 32 {
        panic!("overflow");
    }
    (1 << n) | value
}
```

This is where we set a value of `1` and then shift that one bit left `n` times with the `(1 << n)` then we perform an or operation. This is only one bit so all the bits in the `value` stay in tact apart from the bit in the nth position that then gets flipped on. With this in mind, we can now define the clear bit function with the following code:

```rust
fn clear_bit(value: u32, n: u32) -> u32 {
    if n >= 32 {
        panic!("overflow");
    }
    value & !(1 << n)
}
```

You may have noticed that we're using the `!(1 << n)` which means that the bits are flipped. For instance, if n is 3 the `1 << n` results in `...000001000`. Applying the `!` operator gives us `...111110111`. 

Now that we have a clear bit function, we can move onto a low mask function. A low mask flips all the bits to 1s below a watermark. We can then use and operations to allow only bits below the watermark through, or we can use an and operation with the bits flipped to allow bit above the watermark through. Our low mask function takes the following:

```rust
fn low_mask(width: u32) -> u32 {
    ((1u64 << width) - 1) as u32
}
```

First, the `(1u64 << width)` shifts the `1` to the left `width` times. This means that a width of `3` gives `00001000` once shifted. The `- 1` after the shift collapses the `1` in the bits to `0` and then flips all the bits right of that to `1` resulting in `00000111`. The big idea here is that a low mask lets you treat an integer as a container of individual fields rather than just one.

Now that we have the mask, we can use this to extract multiple smaller numbers or bits out of one compact number with the following code:

```rust
fn extract(value: u32, shift: u32, width: u32) -> u32 {
    let mask = ((1u64 << width) - 1) as u32;
    let shifted = value >> shift;
    shifted & mask
}
```
In this code, we get the mask, we then shift the number to the right, and perform an and operation. The `shifted` defines the starting position of the slice, and the `width` defines the length of the slice. If we have the following:

```text
value = 11010110
shift = 2
width = 3
```
we have the following layout:

```text
Bit index:  7 6 5 4 3 2 1 0
Value:      1 1 0 1 0 1 1 0
                    └───┘
                     3 bits
```
`value >> shift` moves the window to the rightmost position:

```text
Original: 11010110
Shifted:  00110101
```

`& mask` keeps only the width of the window:

```text
Shifted: 00110101
Mask:    00000111
         --------
Result:  00000101
```

Now for inserting that value into the `u32` we have the following function:

```rust
fn insert(value: u32, shift: u32, width: u32, field: u32) -> u32 {
    let mask = ((1u64 << width) - 1) as u32;

    let shifted_mask = mask << shift;
    let cleared = value & !shifted_mask;

    let shifted_field = (field & mask) << shift;

    cleared | shifted_field
}
```

We get the `mask`, we then shift the mask, and then wipe all 