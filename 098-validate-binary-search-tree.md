# [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

## Solution 1. Iterative inorder traversal

Iterative inorder traversal:

**Step 1.** While n != null, stack.push(n), n <- n.left

**Step 2.** While stack.size > 0, n <- stack.pop().right, repeat step 1.

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
  public boolean isValidBST(TreeNode root) {
    if (root == null) return true;
    Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
    for (TreeNode n = root; n != null; n = n.left) stack.addFirst(n);
    TreeNode prev = null;
    while (!stack.isEmpty()) {
      if (prev != null && prev.val >= stack.peekFirst().val) return false;
      prev = stack.removeFirst();
      for (TreeNode n = prev.right; n != null; n = n.left) stack.addFirst(n);
    }
    return true;
  }
}
```

### A more streamlined iterative inorder traversal

[Learn on iterative inorder tree traversal, Leetcode](https://discuss.leetcode.com/topic/46016/learn-one-iterative-inorder-traversal-apply-it-to-multiple-tree-questions-java-solution/2)

Loop invariant:

- Before a node n is pushed into the stack, n and all the child nodes under it haven't been visited yet
- Every node n is visited when it is popped off the stack. By that time, all the left child nodes of n have already been visited, but not the right ones.

```
While n != null or stack.size > 0 do
  while n != null do
    stack.push(n), n <- n.left
  n <- stack.pop()
  visit(n)
  n <- n.right
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
  public boolean isValidBST(TreeNode root) {
    if (root == null) return true;
    Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
    TreeNode prev = null;
    while (root != null || !stack.isEmpty()) {
      while (root != null) {
        stack.addFirst(root);
        root = root.left;
      }
      if (prev != null && prev.val >= stack.peekFirst().val) return false;
      prev = stack.removeFirst();
      root = prev.right;
    }
    return true;
  }
}
```

## Solution 2. Recursive inorder traversal

The catch is, for every node n we need to pass down the previous node to the leftmost child node of n.

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
  public boolean isValidBST(TreeNode root) {
    return isBst(root, null).bst;
  }

  private Result isBst(TreeNode cur, TreeNode prev) {
    // Let the caller, the leftmost node compete with the previous node
    if (cur == null) return new Result(true, prev); 
    Result left = isBst(cur.left, prev);
    if (!left.bst) return left;
    if (left.node != null && left.node.val >= cur.val) return new Result(false, cur);
    return isBst(cur.right, cur);
  }

  private class Result {
    boolean bst;
    TreeNode node;

    Result(boolean b, TreeNode n) {
      bst = b;
      node = n;
    }
  }
}
```

A "cheat" version - track last visited tree node using an object state:

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
  private TreeNode prev = null;

  public boolean isValidBST(TreeNode root) {
    return isBst(root);
  }

  private boolean isBst(TreeNode cur) {
    if (cur == null) {
      return true;
    }
    if (!isBst(cur.left) || (prev != null && prev.val >= cur.val)) return false;
    prev = cur;
    return isBst(cur.right);
  }
}
```

## Solution 3. Lower and upper bounds

For a node n, every node in its left subtree must be smaller than it, and every node in its right subtree must be greater than it. It applies to every node in a BST.

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
  private TreeNode prev = null;

  public boolean isValidBST(TreeNode root) {
    return isInRange(root, null, null);
  }

  private boolean isInRange(TreeNode root, Integer min, Integer max) {
    if (root == null) return true;
    if ((min != null && root.val <= min) || (max != null && root.val >= max)) return false;
    return isInRange(root.left, min, root.val) && isInRange(root.right, root.val, max);
  }
}
```
