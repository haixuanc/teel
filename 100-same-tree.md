# [100. Same Tree](https://leetcode.com/problems/same-tree/)

Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

## Solution 1. Recursive

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
  public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null) return q == null;
    if (q == null || p.val != q.val) return false;
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
  }
}
```

## Solution 2. Iterative

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
  public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null || q == null) return p == q;
    Queue<TreeNode> pairs = new ArrayDeque<TreeNode>();
    pairs.add(p);
    pairs.add(q);
    while (!pairs.isEmpty()) {
      p = pairs.remove();
      q = pairs.remove();
      if (p.val != q.val) return false;
      if (p.left != null && q.left != null) {
        pairs.add(p.left);
        pairs.add(q.left);
      } else if ((p.left == null || q.left == null) && p.left != q.left) {
        return false;
      }
      if (p.right != null && q.right != null) {
        pairs.add(p.right);
        pairs.add(q.right);
      } else if ((p.right == null || q.right == null) && p.right != q.right) {
        return false;
      }
    }
    return true;
  }
}
```

### A refined solution

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
  public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null || q == null) return p == q;
    Queue<TreeNode> pairs = new ArrayDeque<TreeNode>();
    pairs.add(p);
    pairs.add(q);
    while (!pairs.isEmpty()) {
      p = pairs.remove();
      q = pairs.remove();
      if (p.val != q.val) return false;
      if (p.left != null) pairs.add(p.left);
      if (q.left != null) pairs.add(q.left);
      if (pairs.size() % 2 != 0) return false;
      if (p.right != null) pairs.add(p.right);
      if (q.right != null) pairs.add(q.right);
      if (pairs.size() % 2 != 0) return false;
    }
    return true;
  }
}
```
