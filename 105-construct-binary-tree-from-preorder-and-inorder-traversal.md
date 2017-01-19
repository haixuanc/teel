# [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

## Solution 1. Recursion

Preorder sequence: root, [left subtree], [right subtree]

Inorder sequence: [left subtree], root, [right subtree]

f(preorder, inorder) = binary tree for these two sequences

f(preorder, inorder).root = preorder[root]

root.left = f(preorder[left subtree], inorder[left subtree])

root.right = f(preorder[right subtree], inorder[right subtree]) 

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
  public TreeNode buildTree(int[] preorder, int[] inorder) {
    return build(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
  }

  private TreeNode build(int[] preorder, int start1, int end1, int[] inorder, int start2, int end2) {
    if (start1 > end1 || start2 > end2) return null;
    TreeNode root = new TreeNode(preorder[start1]);
    int mid = start2;
    while (mid <= end2 && inorder[mid] != preorder[start1]) mid++;
    root.left = build(preorder, start1 + 1, start1 + mid - start2, inorder, start2, mid - 1);
    root.right = build(preorder, start1 + mid - start2 + 1, end1, inorder, mid + 1, end2);
    return root;
  }
}
```

## Solution 2. Iterative/DFS

If [n1, n2, n3, .., ni, …] is an inorder sequence and [n1, … ni-1] are the nodes in the left subtree of ni, n1 is the leftmost node, n2 is the second left most nodes (n2 is the parent of n1), ….

If [n1, n2, n3, … nj, …] is a preorder sequence and [n2, …, nj] are the nodes in the left subtree of n1, nj is the leftmost node in the subtree, nj-1 is the second leftmost node.

**Loop:** Iterate through each node in preorder [p1, p2, …] until pi equals the first node in inorder sequence [i1, i2, …]. By then, we have reconstruct the leftmost path from the root. Then we pop each node pi off the path and iterate through the inorder sequence until the current node, say pi, does not equal the current inorder number, say i. By then, the next number in the preorder sequence is the right child of the current node.

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
  public TreeNode buildTree(int[] preorder, int[] inorder) {
    if (preorder.length == 0 || preorder.length != inorder.length) return null;
    TreeNode root = new TreeNode(0);
    Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
    TreeNode parent = root;
    boolean left = true;
    for (int i = 0, j = 0; i < preorder.length && j < inorder.length; left = false) {
      while (i < preorder.length && preorder[i] != inorder[j]) {
        stack.addFirst(new TreeNode(preorder[i++]));
        parent = left ? (parent.left = stack.peekFirst()) : (parent.right = stack.peekFirst());
        left = true;
      }
      if (i < preorder.length) {
        stack.addFirst(new TreeNode(preorder[i++]));
        parent = left ? (parent.left = stack.peekFirst()) : (parent.right = stack.peekFirst());
        left = true;
      }
      while (j < inorder.length && !stack.isEmpty() && inorder[j] == stack.peekFirst().val) {
        parent = stack.removeFirst();
        j++;
      }
    }
    return root.left;
  }
}
```
