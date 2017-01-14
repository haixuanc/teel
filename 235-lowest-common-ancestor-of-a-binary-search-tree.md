# [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

_______6______
/              \
    ___2__          ___8__
    /      \        /      \
      0      _4       7       9
      /  \
        3   5

For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.

## Solution 1. Top-down recursion

Time: O(lgn)

Space: O(1)

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
    if (root == null) return null;
    if (p == null) return q;
    if (q == null) return p;
    if (p == root || q == root) return root;
    if ((p.val <= root.val && q.val >= root.val) || (q.val <= root.val && p.val >= root.val)) return root;
    if (p.val <= root.val && q.val <= root.val) return lowestCommonAncestor(root.left, p, q);
    return lowestCommonAncestor(root.right, p, q);
  }
}
```

More streamlined versions:

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
    if (root == null) return root;
    if (p == null) return q;
    if (q == null) return p;
    if ((root.val - p.val) * (root.val - q.val) <= 0) return root;
    if (p.val < root.val) return lowestCommonAncestor(root.left, p, q);
    return lowestCommonAncestor(root.right, p, q);
  }
}
```

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
    if (root == null) return root;
    if (p == null) return q;
    if (q == null) return p;
    if (root.val < Math.min(p.val, q.val)) return lowestCommonAncestor(root.right, p, q);
    if (root.val > Math.max(p.val, q.val)) return lowestCommonAncestor(root.left, p, q);
    return root;
  }
}
```

## Solution 2. Iterative DFS

Time: O(lgn)

Space: O(1)

**NOTE:** since the binary search tree itself is a recursive data structure, a *recursive* algorithm usually can be implemented *iteratively* by recursively updating the data structure.

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
    while (root != null && (root.val - p.val) * (root.val - q.val) > 0) {
      root = root.val < p.val ? root.right : root.left;
    }
    return root;
  }
}
```

## Applications of Lowest Common Ancestor (LCA)

For two nodes v and w in a tree or directed acyclic graph, the distance from v to w can be computed as:

d(v, w) = d(v, root) + d(w, root) - 2 * d(lca, root)
