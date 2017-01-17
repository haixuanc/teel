# [449. Serialize and Deserialize BST](https://leetcode.com/problems/serialize-and-deserialize-bst/)

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary search tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.

The encoded string should be as compact as possible.

Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

## Solution 1. Level by level encoding and decoding/BFS

This algorithm applies to all **binary trees** in general not just **binary search trees**. But we have to encode leaf nodes as well to correctly mark the end of each subtree.

Time: O(2n), it encodes all n tree nodes and O(n) extra "null nodes" under all leaf nodes

Space: O(2n)

```java
/**
 * Definition for a binary tree node.
 *  * public class TreeNode {
 *   *     int val;
 *    *     TreeNode left;
 *     *     TreeNode right;
 *      *     TreeNode(int x) { val = x; }
 */
public class Codec {

  // Encodes a tree to a single string.
  public String serialize(TreeNode root) {
    if (root == null) return "";
    StringBuilder code = new StringBuilder();
    code.append(root.val);
    Queue<TreeNode> level = new LinkedList<TreeNode>();
    level.add(root);
    while (!level.isEmpty()) {
      int last = level.size();
      while (last-- > 0) {
        TreeNode n = level.remove();
        if (n.left != null) {
          code.append("," + n.left.val);
          level.add(n.left);
        } else {
          code.append(",");
        }
        if (n.right != null) {
          code.append("," + n.right.val);
          level.add(n.right);
        } else {
          code.append(",");
        }
      }
    }
    return code.toString();
  }

  // Decodes your encoded data to tree.
  public TreeNode deserialize(String data) {
    if (data.length() == 0) return null;
    String[] nodes = data.split(",");
    if (nodes.length == 0) return null;
    TreeNode root = new TreeNode(Integer.parseInt(nodes[0]));
    Queue<TreeNode> level = new LinkedList<TreeNode>();
    level.add(root);
    int i = 1;
    while (!level.isEmpty() && i < nodes.length) {
      TreeNode n = level.remove();
      if (nodes[i].length() > 0) {
        n.left = new TreeNode(Integer.parseInt(nodes[i]));
        level.add(n.left);
      }
      if (i + 1 < nodes.length && nodes[i + 1].length() > 0) {
        n.right = new TreeNode(Integer.parseInt(nodes[i + 1]));
        level.add(n.right);
      }
      i += 2;
    }
    return root;
  }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

## Solution 2. Only encode existent BST tree nodes/DFS encoding

The advantage of solution 1 is that it works for all **binary tree** not just **binary search trees**. However that algorithm has to encode leaf nodes, which costs extra space.

If we are given a sequence of **binary search tree** nodes in preorder, we can re-construct the BST recursively. It utilizes the property of BST that for each node r all nodes is its left subtree are smaller that r and all nodes in its right subtree are greater than r. This information will be "encoded" if we traverse the tree nodes in preorder.

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
public class Codec {

  // Encodes a tree to a single string.
  public String serialize(TreeNode root) {
    if (root == null) return "";
    StringBuilder preorder = new StringBuilder();
    Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
    stack.addFirst(root);
    while (!stack.isEmpty()) {
      TreeNode n = stack.removeFirst();
      preorder.append(n.val).append(",");
      if (n.right != null) stack.addFirst(n.right);
      if (n.left != null) stack.addFirst(n.left);
    }
    return preorder.toString();
  }

  // Decodes your encoded data to tree.
  public TreeNode deserialize(String data) {
    if (data.length() == 0) return null;
    String[] preorder = data.split(",");
    return bst(preorder, 0, preorder.length - 1);
  }

  private TreeNode bst(String[] preorder, int start, int end) {
    if (start > end) return null;
    TreeNode root = new TreeNode(Integer.parseInt(preorder[start]));
    int mid = start + 1;
    while (mid <= end && Integer.parseInt(preorder[mid]) < root.val) mid++;
    root.left = bst(preorder, start + 1, mid - 1);
    root.right = bst(preorder, mid, end);
    return root;
  }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```
