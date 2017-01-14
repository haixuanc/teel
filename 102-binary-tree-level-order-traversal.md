# [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree [3,9,20,null,null,15,7],

      3
      / \
        9  20
        /  \
          15   7

return its level order traversal as:

[
  [3],
  [9,20],
  [15,7]
]

## Solution 1. Iterative BFS

Time: O(n)

Space: O(n / 2), 2^h = 2^(lgn - 1) = n/2

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
  public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> levels = new ArrayList<List<Integer>>();
    Queue<TreeNode> next = new ArrayDeque<TreeNode>();
    if (root != null) next.add(root);
    while (!next.isEmpty()) {
      levels.add(new ArrayList<Integer>());
      int current = next.size();
      while (current-- > 0) {
        TreeNode p = next.remove();
        levels.get(levels.size() - 1).add(p.val);
        if (p.left != null) next.add(p.left);
        if (p.right != null) next.add(p.right);
      }
    }
    return levels;
  }
}
```

## Solution 2. Recursive DFS

Time: O(n)

Space: O(n)

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
  public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> levels = new ArrayList<List<Integer>>();
    dfs(levels, 0, root);
    return levels;
  }

  private void dfs(List<List<Integer>> levels, int level, TreeNode cur) {
    if (cur == null) return;
    if (level == levels.size()) levels.add(new ArrayList<Integer>());
    levels.get(level++).add(cur.val);
    dfs(levels, level, cur.left);
    dfs(levels, level, cur.right);
  }
}
```
