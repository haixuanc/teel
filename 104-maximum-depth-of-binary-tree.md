# [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

### Solution 1. DFS/Recursion

Time: O(n)
Space: avg: O(lgn), worst: O(n) the tree is a chain

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

### Solution 2. BFS/Iterative

Time: O(n)
Avg space: O(n), for a balanced BT the number of leaf nodes is half of the total number of nodes

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        Queue<TreeNode> level = new LinkedList<>();
        level.add(root);
        int max = 0;
        while (!level.isEmpty()) {
            max++;
            for (int size = level.size(); size > 0; size--) {
                TreeNode cur = level.remove();
                if (cur.left != null) level.add(cur.left);
                if (cur.right != null) level.add(cur.right);
            }
        }
        return max;
    }
}
```
