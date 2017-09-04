# [112. Path Sum](https://leetcode.com/problems/path-sum/description/)

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and sum = 22,

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

## DFS recursion

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
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) return false; // hasPathSum(null, 0) -> false
        if (root.left == null && root.right == null) return root.val == sum;
        sum -= root.val;
        return hasPathSum(root.left, sum) || hasPathSum(root.right, sum);
    }
}
```

## BFS iteration

Time: O(n)

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
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) return false; // hasPathSum(null, 0) -> false
        Queue<TreeNode> level = new LinkedList<>();
        root.val -= sum;
        level.add(root);
        while (!level.isEmpty()) {
            for (int size = level.size(); size > 0; size--) {
                TreeNode cur = level.remove();
                if (cur.left == null && cur.right == null && cur.val == 0) return true;
                if (cur.left != null) {
                    cur.left.val += cur.val;
                    level.add(cur.left);
                }
                if (cur.right != null) {
                    cur.right.val += cur.val;
                    level.add(cur.right);
                }
            }
        }
        return false;
    }
}
```
