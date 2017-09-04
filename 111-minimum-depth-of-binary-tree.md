# [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

## BFS

Time: O(lgn)

Space: O(n)

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
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        Queue<TreeNode> level = new LinkedList<>();
        level.add(root);
        int depth = 0;
        while (!level.isEmpty()) {
            depth++;
            for (int size = level.size(); size > 0; size--) {
                TreeNode cur = level.remove();
                if (cur.left == null && cur.right == null) return depth;
                if (cur.left != null) level.add(cur.left);
                if (cur.right != null) level.add(cur.right);
            }
        }
        return depth;
    }
}
```

## DFS

Time: O(n)

Space: O(lgn)

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
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        return 1 + ((left == 0 || right == 0) ? left + right : Math.min(left, right));
    }
}
```
