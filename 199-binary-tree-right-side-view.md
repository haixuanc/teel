# [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

For example:
Given the following binary tree,

      1            <---
    /   \
   2     3         <---
    \     \
      5     4       <---

You should return [1, 3, 4].

## Solution 1. BFS

Traverse the tree nodes by level and print the last node at each level.

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
  public List<Integer> rightSideView(TreeNode root) {
    if (root == null) return new ArrayList<Integer>();
    List<Integer> rightmost = new ArrayList<Integer>();
    Queue<TreeNode> level = new ArrayDeque<TreeNode>();
    level.add(root);
    while (!level.isEmpty()) {
      int old = level.size();
      TreeNode last = null;
      while (old-- > 0) {
        last = level.remove();
        if (last.left != null) level.add(last.left);
        if (last.right != null) level.add(last.right);
      }
      rightmost.add(last.val);
    }
    return rightmost;
  }
}
```

## Solution 2. DFS

Traverse the tree in preorder (right child node first) and only add current node to the result list if current depth is greater than the size of the result list at the moment.

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
  public List<Integer> rightSideView(TreeNode root) {
    if (root == null) return new ArrayList<Integer>();
    List<Integer> rightmost = new ArrayList<Integer>();
    dfs(rightmost, 0, root);
    return rightmost;
  }

  private void dfs(List<Integer> rightmost, int depth, TreeNode root) {
    if (root == null) return;
    if (depth == rightmost.size()) rightmost.add(root.val);
    dfs(rightmost, depth + 1, root.right);
    dfs(rightmost, depth + 1, root.left);
  }
}
```
