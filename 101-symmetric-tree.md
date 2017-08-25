## 101. Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

But the following [1,2,2,null,3,null,3] is not:

```
    1
   / \
  2   2
   \   \
   3    3
```

Note:
Bonus points if you could solve it both recursively and iteratively.

## Solution 1. Recursion

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
public class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        return isMirror(root.left, root.right);
    }

    private boolean isMirror(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null) return true;
        if (t1 == null || t2 == null || t1.val != t2.val) return false;
        return isMirror(t1.left, t2.right) && isMirror(t1.right, t2.left);
    }
}
```

## Solution 2. Iterative

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
    public boolean isSymmetric(TreeNode root) {
        if (root == null || (root.left == null && root.right == null)) return true;
        Deque<TreeNode> pairs = new LinkedList<>();
        if (root.left == null || root.right == null || root.left.val != root.right.val) return false;
        pairs.push(root.left);
        pairs.push(root.right);
        while (!pairs.isEmpty()) {
            TreeNode left = pairs.pop();
            TreeNode right = pairs.pop();
            if (left.left != null && right.right != null) {
                if (left.left.val == right.right.val) {
                    pairs.push(left.left);
                    pairs.push(right.right);
                } else {
                    return false;
                }
            } else if (!(left.left == null && right.right == null)) {
                return false;
            }
            if (left.right != null && right.left != null) {
                if (left.right.val == right.left.val) {
                    pairs.push(left.right);
                    pairs.push(right.left);
                } else {
                    return false;
                }
            } else if (!(left.right == null && right.left == null)) {
                return false;
            }
        }
        return true;
    }
}
```
