---
tags:
  - algorithms
  - skiena
  - hashing
  - data-structures
source: Skiena, The Algorithm Design Manual, §3.7
written: 2026-10-31
---

# Hashing

## Hash functions

A hash function is essentially a function that converts a key into an integer. For the equation below we have a hash function for converting a string into a number:

$$
H(S) = \alpha^{|S|} + \sum_{i=0}^{|S|-1} \alpha^{\,|S|-(i+1)} \times \mathrm{char}(S_i)
$$

$S$ is the string, $\alpha$ is the total number of letters in the alphabet, and $\mathrm{char}$ is a function that converts a character to a number. $|S|$ is just the length of the string. For example, let's hash the string `"abc"`:

$$
H(abc) = 26^{3} + \left(26^{3-1} \times 0\right) + \left(26^{3-2} \times 1\right) + \left(26^{3-3} \times 2\right)
$$

where $|S| = 3$, $\alpha = 26$ and the char function is zero-based:

$$
a = 0,\quad b = 1,\quad c = 2
$$

We can simplify this to:

$$
H(abc) = 26^{3} + 0 + 26 + 2 = 17{,}604
$$

> [!note] Changed from the handwritten version
> The page used $a=1, b=2, c=3$, giving $18{,}307$. Per the "note to LLM" it has been shifted to zero-based ($a=0, b=1, c=2$), which gives $17{,}604$. Both are correct under their own char mapping; the index below has been recomputed to match.

> [!note] Check against Skiena
> Skiena's string hash is just the sum, $H(S) = \sum_{i=0}^{|S|-1} \alpha^{|S|-(i+1)}\,\mathrm{char}(S_i)$. The leading $\alpha^{|S|}$ term looks like an addition of yours. It's harmless (it's a constant for a given length, so it doesn't hurt distribution) but worth checking against the book.

## Reducing to a bucket index

We can see that $17{,}604$ is high just for `"abc"`. Larger strings will be even bigger. Therefore, we need to take the modulo. Let's say that we have a slice of 4 buckets. We can calculate the index of the key with the following:

$$
I_4\big(H(abc)\big) = 17{,}604 \bmod 4 = 0
$$

The modulo is the remainder, so the modulo of 4 can only be $0, 1, 2, 3$. If it ties over to 4 then there is no remainder, therefore it's zero again. (That's exactly what happened here: $17{,}604 = 4 \times 4{,}401$, so it lands on bucket 0.) The modulo is a great trick for indexing and queues. However, be careful: modulo is division and it is expensive.

> [!tip] Power-of-two trick
> If the number of buckets is a power of two, $m = 2^k$, then $h \bmod m$ is the same as $h \,\&\, (m-1)$, a single AND instruction instead of a division. This is why most hash table implementations size their bucket array as a power of two.

## Collisions

Because we don't have arrays with infinite size, hashes will collide when trying to allocate a bucket to a hash. One way of solving this is having chains off each bucket so we can add the colliding elements in the chain. However, this takes extra memory (a pointer per bucket and a node per element) that could instead have been used to make the table bigger and reduce the collisions.

**Open addressing** is where we probe by moving the pointer up one if there's a collision. This keeps everything in one contiguous array, which is good for caching. Deletion is done by tombstoning the slot rather than removing it (otherwise probe sequences would break), but then you have compaction as tombstones build up.

> [!note] Clarified
> The handwritten line "this makes the caching and deletion (if tombstoned)" was garbled; the above is the intended meaning: open addressing is cache-friendly, deletion needs tombstones, tombstones eventually force a rehash/compaction.

Chaining and probing both cost $O(m)$ to initialise, where $m$ is the number of buckets.

## Other applications of hashing

Hashing has other applications. The idea of a hash is to represent a large object such as a string by a large number. It is very unlikely that two different large objects will result in the same number.

- **Duplicate detection.** Hashing an entire webpage is a quick way of finding duplicate webpages.
- **Plagiarism detection.** If we wanted to detect plagiarism, we can have a hash table of overlapping windows of the document, and look for windows whose hashes appear in both.
- **Change detection.** Cryptographic hashing is also used to detect changes in network packets or files (integrity checks: if the hash differs, something changed).
- **Canonicalisation / fingerprinting.** We can also use hashing for canonicalisation, or compaction such as fingerprinting: a short fixed-size value that stands in for the big object.

## Related

- [[Hash map collision handling]] — open addressing vs chaining, load factor, hashbrown