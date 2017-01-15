# [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

```
            3______
      /              \
    ___5__          ___1__
    /      \        /      \
    6      _2       0       8
          /  \
          7   4
```

For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

## Solution 1. Recursion

Time: O(n)

Space: O(lgn)

We assume p and q are not null, and p and q must be in the tree.

```java
/**
 * Definition for a binary tree node.
 *  * public class TreeNode {
 *   *     int val;
 *    *     TreeNode left;
 *     *     TreeNode right;
 *      *     TreeNode(int x) { val = x; }
 */
public class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    if (left != null && left != p && left != q) return left;
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    return left != null && right != null ? root : left != null ? left : right;
  }
}
```

## Solution 2. Iterative

**Step 1.** We will first exhaust the leftmost path of the tree by traversing down in pre-order (DFS). Then we iterate through each node on the path from the leaf to the root until the current node is either p or q, or we will search for p or q in the right sub-tree rooted at current node. If p or q is never found, there is no LCA for them.

If we assume p and q are both in the tree, i.e. there is a LCA for them, there will be two possible cases:

I. p is the ancestor for q, or q is the ancestor for p
II. some node r is the lca for q and p; p and q are in the two different sub-trees rooted at r

- If it is case I, we will meet the lower one of p and q in step 1.
- If it is case II, we will meet the left one of p and q in step 1.

**Step 2.** For example, we have found p in step 1. Then we will iterate through each node r on the path [p, root]. If r equals q which is case I, or q is the right sub-tree of r which is case II, r is the LCA for p and q. If q is never found, there is no LCA for them.

Time: O(n)

Space: O(lgn)

```java
/**
 * Definition for a binary tree node.
 *  * public class TreeNode {
 *   *     int val;
 *    *     TreeNode left;
 *     *     TreeNode right;
 *      *     TreeNode(int x) { val = x; }
 */
public class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;
    Deque<TreeNode> parents = new ArrayDeque<TreeNode>();
    while (root != null) {
      parents.addFirst(root);
      root = root.left;
    }
    while (!parents.isEmpty()) {
      TreeNode n = parents.peekFirst();
      if (n == p || n == q) break;
      n = parents.removeFirst().right;
      while (n != null) {
        parents.addFirst(n);
        n = n.left;
      }
    }
    if (parents.isEmpty()) return null;
    TreeNode t = parents.peekFirst() == p ? q : p;
    while (!parents.isEmpty()) {
      Deque<TreeNode> sub = new ArrayDeque<TreeNode>();
      root = parents.removeFirst();
      sub.addFirst(root);
      while (!sub.isEmpty()) {
        TreeNode n = sub.removeFirst();
        if (n == t) return root;
        n = n.right;
        while (n != null) {
          sub.addFirst(n);
          n = n.left;
        }
      }
    }
    return null;
  }
}
```
