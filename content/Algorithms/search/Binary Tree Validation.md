---
tags: [algorithms, trees, bst, leetcode]
related: "[[introduction to algorithms]]"
created: 2026-11-02
---

# Validating a Binary Search Tree

A valid binary search tree has the following rules:

1. The left subtree of a node contains only nodes with keys **less than** the node's key.
2. The right subtree of a node contains only nodes with keys **greater than** the node's key.
3. Both the left and right subtrees must also be binary search trees.

So to illustrate a nuance, let's look at the following tree:

![Binary search tree rooted at 5, each node labelled with the range it is allowed to take; 4 sits under 7 with allowed range (5, 7), so the tree is invalid](_attachments/bst-validation-ranges.png)

```
        5 (−∞, ∞)
       / \
(−∞,5) 3   7 (5, ∞)
          / \
   (5,7) 4   8 (7, ∞)
```

If we look at each node individually (i.e. only comparing a node to its immediate parent) we might think that it's valid. However, 4 is smaller than 5, so the 4 should be on the right of 3, not under 7.

Instead, a good approach is keeping track of a **range**. At the root, the value can be anything, so the range is $(-\infty, \infty)$. We then move left: the node on the left is 3 and we now have the range $-\infty < 3 < 5$. When we get to the node 7, the range is $(5, \infty)$. For node 8 we have $(7, \infty)$, and for node 4 the allowed range is $(5, 7)$ — which 4 is not in, so the tree is invalid.

In general, going left tightens the **upper** bound to the parent's value, and going right tightens the **lower** bound to the parent's value.

We can validate a tree with a recursive function:

```python
def validate(node, min, max):
    if node is None:
        return True

    if min is not None and node.val <= min:
        return False
    if max is not None and node.val >= max:
        return False

    return (validate(node.left, min, node.val)
        and validate(node.right, node.val, max))


# initial call
validate(root, None, None)
```

We can now solve the problem in Rust. First we must define the tree node with the following code:

```rust
#[derive(Debug, PartialEq, Eq)]
pub struct TreeNode {
  pub val: i32,
  pub left: Option<Rc<RefCell<TreeNode>>>,
  pub right: Option<Rc<RefCell<TreeNode>>>,
}
```

Here, we can see that the left and right pointers of the nodes are optional, and then they are wrapped in an `Rc` which is a reference counter that is None thread safe. This means that we can have shared ownership of the child nodes. The `RefCell` gives us internal mutability so we can have multiple references to the `Rc` layer and then still have one reference call a `borrow_mut` on the `RefCell` to mutate the node as long as none of the references are calling `borrow` or `borrow_mut` on the `RefCell` at the same time. With the referencing cleared up, our validation takes the following form:

```rust
fn validate(node: &Option<Rc<RefCell<TreeNode>>>, min: Option<i32>, max: Option<i32>) -> bool {

    // If the Node is None this is automatically valid 
    let node = match node {
        Some(node) => node,
        None => return true,
    };

    // get the value from an imutable borrow
    let borrow = node.borrow();
    let val = borrow.val;

    // return false if the node bypasses the ranges needed to be valid
    if min.is_some_and(|m| val <= m) || max.is_some_and(|m| val >= m) {
        return false;
    }
    // Make the recursive call for the left and right children of the node
    validate(&borrow.left, min, Some(val)) && validate(&borrow.right, Some(val), max)
}
```

Notes:

- `None` for `min`/`max` stands in for $-\infty$ / $\infty$, which avoids having to pick sentinel values that might collide with real keys.
- The comparisons are `<=` and `>=` because the rules say *strictly* less than / greater than, so duplicates are not allowed.
- Every node is visited once, so this is $O(n)$ time and $O(h)$ space for the recursion stack, where $h$ is the tree height.

## Connections

- The worked counterexample [[introduction to algorithms]] asks for. Checking each node against its immediate parent is a plausible algorithm that passes this tree, and node 4 is the single example that sinks it — no amount of trees it does classify correctly would have told you. The range version is what a *well specified* statement of the rules looks like: every key in the left subtree, not merely the left child.
- The range is a set the key must belong to, written in the interval notation of [[sets]]: $(-\infty, \infty)$ at the root, and each step down intersects it with a half-line. Going left intersects with $(-\infty, \text{parent})$ and going right with $(\text{parent}, \infty)$, so a node's allowed range is the intersection of every constraint on the path from the root — which is why it can only ever tighten.
- `validate` is a recurrence with a base case, in the sense of [[basic recursion for sum loop]], and the empty tree returning `True` is that note's empty-sum convention: the identity element of AND, so a leaf's two missing children contribute nothing. Proving the range test equivalent to the subtree definition is structural induction over the tree rather than over $n$.
- The traversal is DFS, the depth-first half of [[Breadth First Search]]. What is unusual is the state: the three hooks there carry no information downwards, whereas here each call hands its children a tightened range, so the invariant travels with the recursion instead of living in a `visited` set. Swap the recursion for a queue and the ranges still work — correctness does not depend on the order, only on the inheritance.
- An in-order traversal of a BST is the keys in sorted order, so validity has a second formulation: the sequence must be strictly increasing. That is a monotonic sweep in the sense of [[Monotonic Stack]] — keep the previous key, fail if the next does not exceed it — and the iterative version of this note is literally that note's explicit stack with an in-order discipline. Two readings of one invariant: a bound passed down, or an order checked across.
- $O(h)$ space, and $h = n$ in the worst case, is the degenerate tree where every node has one child — which is a [[Linked Lists|linked list]], the same degeneracy [[Breadth First Search]] notes for a graph with out-degree one. Balancing exists to keep $h$ at $\log n$; without it a "tree" can quietly become a chain, and the recursion a stack overflow.
- `None` standing in for $\pm\infty$ rather than `i32::MIN`/`i32::MAX` is the sentinel question that runs through the vault: reserve a value outside the key domain, as the all-zero id in [[Bit Encoding]] means "nothing". Picking an in-domain sentinel is how a legitimate key equal to `i32::MIN` turns into a false negative.
- Ordered against unordered membership: [[Hashing]] answers "is this key present" in expected $O(1)$ but destroys order, while a BST pays $O(h)$ and keeps it, which is what makes range queries and sorted iteration possible at all. The on-disk version of that same trade is the B-tree, the reason databases index with them rather than hash tables — [[Trade-offs in Data Systems Architecture]].
- Part of [[Home]].

## Open

- [ ] The iterative in-order version with an explicit stack, written against the recursive one, and the "strictly increasing" check as a monotonic sweep ([[Monotonic Stack]]).
- [ ] The structural induction that the range invariant and the subtree definition are the same statement, in the style of [[basic recursion for sum loop]].
- [ ] Search, insert and delete on a BST, and why each is $O(h)$ by the same argument the ranges make.
- [ ] Balancing: AVL and red-black rotations, and what keeps $h$ at $\Theta(\log n)$ so the degenerate chain above cannot happen.
- [ ] Rust implementation with the LeetCode `Option<Rc<RefCell<TreeNode>>>` shape, and how `Option` bounds avoid the `i32::MIN` sentinel trap.
- [ ] B-trees as the paged generalisation, once the Ch4 notes in [[Trade-offs in Data Systems Architecture]] are written.
