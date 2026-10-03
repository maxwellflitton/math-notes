Below is the test code:
```rust
//! Bit-twiddling ladder. Run `cargo test`. Fill the `todo!()`s from the top down.
//!
//! Only four tools are needed for everything here:
//!   <<  shift left     (multiply by 2^n, move a field up)
//!   >>  shift right    (divide by 2^n, move a field down)
//!   &   AND            (keep only the bits you mask; test a bit)
//!   |   OR             (switch bits on; combine fields that do not overlap)
//!   !   NOT            (flip every bit; with & it clears bits)
//!   ^   XOR            (flip only the bits you name)

// ---------- Rung 1: one bit at a time ----------

/// Is bit `n` (0 = least significant) set?
fn get_bit(value: u32, n: u32) -> bool {
    todo!()
}

/// Return `value` with bit `n` switched on.
fn set_bit(value: u32, n: u32) -> u32 {
    todo!()
}

/// Return `value` with bit `n` switched off. Hint: AND with the NOT of a single bit.
fn clear_bit(value: u32, n: u32) -> u32 {
    todo!()
}

/// Return `value` with bit `n` flipped.
fn toggle_bit(value: u32, n: u32) -> u32 {
    todo!()
}

// ---------- Rung 2: fields, not bits ----------

/// A mask with the lowest `width` bits set: width 3 -> 0b111. Beware width 32.
fn low_mask(width: u32) -> u32 {
    todo!()
}

/// Read a `width`-bit field that starts at bit `shift`.
fn extract(value: u32, shift: u32, width: u32) -> u32 {
    todo!()
}

/// Write `field` into the `width`-bit slot at `shift`, leaving every other bit alone.
/// Hint: clear the slot first, then OR the field in. `field` may have junk above `width`.
fn insert(value: u32, shift: u32, width: u32, field: u32) -> u32 {
    todo!()
}

// ---------- Rung 3: the engine's record id ----------

const KIND_SHIFT: u32 = 29; // three kind bits on top, 29 slot bits below

fn pack_id(kind: u32, slot: u32) -> u32 {
    todo!()
}

fn kind_of(id: u32) -> u32 {
    todo!()
}

fn slot_of(id: u32) -> u32 {
    todo!()
}

// ---------- Rung 4: the id texture's halves ----------

/// Split into (low 16 bits, high 16 bits), as `pack_ids` does for the Rgba16Uint texture.
fn split_halves(value: u32) -> (u16, u16) {
    todo!()
}

fn join_halves(low: u16, high: u16) -> u32 {
    todo!()
}

// ---------- Rung 5: the packed hover colour ----------

fn pack_rgb8([r, g, b]: [f32; 3]) -> u32 {
    todo!()
}

/// The shader's side of `pack_rgb8`.
fn unpack_rgb8(packed: u32) -> [f32; 3] {
    todo!()
}

// ---------- Rung 6: one-hot roles and the highlight band ----------

/// Exactly one bit set? Hint: `x & (x - 1)` clears the lowest set bit.
fn is_one_hot(x: u32) -> bool {
    todo!()
}

/// Count set bits with a loop, without calling `count_ones`.
fn popcount(mut x: u32) -> u32 {
    todo!()
}

const HIGHLIGHT_SHIFT: u32 = 1;

/// One bit per kind number, starting at HIGHLIGHT_SHIFT, as `update_highlight_kinds` builds.
fn band_from_kinds(kinds: &[u32]) -> u32 {
    todo!()
}

fn band_admits(band: u32, kind: u32) -> bool {
    todo!()
}

// ---------- Rung 7: the sharp edges ----------

/// Shift a signed number right. Think first: what does -8 >> 1 give, and why is it not
/// the same as (-8i32 as u32) >> 1? Return both so the test shows you.
fn signed_vs_logical(x: i32) -> (i32, u32) {
    todo!()
}

/// Reverse the bit order of a byte: 0b0000_0001 -> 0b1000_0000.
fn reverse_byte(mut b: u8) -> u8 {
    todo!()
}

fn main() {
    println!("run `cargo test`");
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn rung_1_single_bits() {
        assert!(get_bit(0b1000, 3));
        assert!(!get_bit(0b1000, 2));
        assert_eq!(0b1001, set_bit(0b1000, 0));
        assert_eq!(0b1000, set_bit(0b1000, 3), "setting a set bit changes nothing");
        assert_eq!(0b0000, clear_bit(0b1000, 3));
        assert_eq!(0b1000, clear_bit(0b1000, 0), "clearing a clear bit changes nothing");
        assert_eq!(0b1001, toggle_bit(0b1000, 0));
        assert_eq!(0b0000, toggle_bit(0b1000, 3));
        assert_eq!(0x8000_0000, set_bit(0, 31), "the top bit");
    }

    #[test]
    fn rung_2_fields() {
        assert_eq!(0b111, low_mask(3));
        assert_eq!(0, low_mask(0));
        assert_eq!(u32::MAX, low_mask(32), "the whole word, without overflowing 1 << 32");
        // 0xAB in bits 8..16 of 0x00AB_CD
        assert_eq!(0xAB, extract(0x00AB_CD, 8, 8));
        assert_eq!(0xCD, extract(0x00AB_CD, 0, 8));
        assert_eq!(0b101, extract(0b1_0101_1000, 3, 3));
        assert_eq!(0x00EF_CD, insert(0x00AB_CD, 8, 8, 0xEF));
        assert_eq!(0x00AB_CD, insert(0x00AB_CD, 8, 8, 0xAB), "same field, same word");
        assert_eq!(0x0001_CD, insert(0x00AB_CD, 8, 8, 0x1_01), "junk above the width is dropped");
    }

    #[test]
    fn rung_3_ids() {
        let id = pack_id(6, 5);
        assert_eq!(0xC000_0005, id);
        assert_eq!(6, kind_of(id));
        assert_eq!(5, slot_of(id));
        assert_eq!(0, kind_of(pack_id(0, 0)), "kind None, slot 0 is the zero id");
        assert_eq!((1 << 29) - 1, slot_of(pack_id(7, u32::MAX)), "a slot cannot spill into the kind");
    }

    #[test]
    fn rung_4_halves() {
        assert_eq!((0xCD05, 0xC000), split_halves(0xC000_CD05));
        assert_eq!(0xC000_CD05, join_halves(0xCD05, 0xC000));
        for id in [0, 1, 0xFFFF, 0x1_0000, u32::MAX, 0xC000_0005] {
            let (low, high) = split_halves(id);
            assert_eq!(id, join_halves(low, high), "round trip {id:#x}");
        }
    }

    #[test]
    fn rung_5_colour() {
        assert_eq!(0xFF0000, pack_rgb8([1.0, 0.0, 0.0]));
        assert_eq!(0x00FF00, pack_rgb8([0.0, 1.0, 0.0]));
        assert_eq!(0x0000FF, pack_rgb8([0.0, 0.0, 1.0]));
        assert_eq!(0x333333, pack_rgb8([0.2, 0.2, 0.2]));
        assert_eq!(0xFFFFFF, pack_rgb8([9.0, 1.0, 2.0]), "clamped");
        let back = unpack_rgb8(0x333333);
        for c in back {
            assert!((c - 0.2).abs() < 1.0 / 255.0, "within one step of a byte");
        }
        assert_eq!([1.0, 0.0, 1.0], unpack_rgb8(0xFF00FF));
    }

    #[test]
    fn rung_6_roles_and_band() {
        assert!(is_one_hot(1 << 9));
        assert!(!is_one_hot(0b11));
        assert!(!is_one_hot(0), "zero has no bit at all");
        assert_eq!(0, popcount(0));
        assert_eq!(32, popcount(u32::MAX));
        assert_eq!(3, popcount(0b1011_0000));

        let band = band_from_kinds(&[1, 2, 5]);
        assert_eq!(0b0100_1100, band, "bits 2, 3 and 6: kind + HIGHLIGHT_SHIFT");
        assert!(band_admits(band, 1));
        assert!(band_admits(band, 5));
        assert!(!band_admits(band, 3));
        assert_eq!(0, band_from_kinds(&[]));
    }

    #[test]
    fn rung_7_sharp_edges() {
        // Arithmetic shift keeps the sign: -8 >> 1 is -4. Logical shift on the same bits
        // drags a zero in at the top and gives a huge positive number.
        assert_eq!((-4, 0x7FFF_FFFC), signed_vs_logical(-8));
        assert_eq!(0b1000_0000, reverse_byte(0b0000_0001));
        assert_eq!(0b0001_0011, reverse_byte(0b1100_1000));
        assert_eq!(0xFF, reverse_byte(0xFF));
    }
}
```