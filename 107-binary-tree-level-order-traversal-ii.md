# [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/description/)

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree [3,9,20,null,null,15,7],

```
    3
   / \
  9  20
    /  \
   15   7
```

return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]
```

## Solution 1. Level by level traversal/BFS

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> levels = new ArrayList<List<Integer>>();
        if (root == null) return levels;
        Queue<TreeNode> level = new LinkedList<>();
        level.add(root);
        while (!level.isEmpty()) {
            levels.add(0, new ArrayList<Integer>());
            level.stream().forEach(n -> levels.get(0).add(n.val));
            for (int size = level.size(); size > 0; size--) {
                TreeNode cur = level.remove();
                if (cur.left != null) level.add(cur.left);
                if (cur.right != null) level.add(cur.right);
            }
        }
        return levels;
    }
}
```

## Solution 2. DFS

Time: O(n)

Space: O(n)

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrderBottom = function(root) {
    var levels = [];
    dfs(levels, 0, root);
    return levels;
};

var dfs = function(levels, depth, node) {
    if (node === null) return;
    if (++depth > levels.length) {
        levels.unshift([]);
    }
    dfs(levels, depth, node.left);
    dfs(levels, depth, node.right);
    levels[levels.length - depth].push(node.val);
};
```

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> levels = new ArrayList<>();
        dfs(levels, 0, root);
        return levels;
    }

    private void dfs(List<List<Integer>> levels, int depth, TreeNode node) {
        if (node == null) return;
        if (++depth > levels.size()) levels.add(0, new ArrayList<Integer>());
        dfs(levels, depth, node.left);
        dfs(levels, depth, node.right);
        levels.get(levels.size() - depth).add(node.val);
    }
}
```
