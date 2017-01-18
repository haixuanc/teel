# [297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following tree

```
     1
    / \
   2   3
      / \
     4   5
```

as "[1,2,3,null,null,4,5]", just the same as how LeetCode OJ serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

## Solution 1. Level by level traversal/BFS

The catch is we need to encode "null nodes" under each leaf nodes as well.

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
    StringBuilder code = new StringBuilder();
    code.append(root.val);
    Queue<TreeNode> remaining = new LinkedList<TreeNode>();
    remaining.add(root);
    while (!remaining.isEmpty()) {
      TreeNode n = remaining.remove();
      code.append(",");
      if (n.left != null) {
        code.append(n.left.val);
        remaining.add(n.left);

      }
      code.append(",");
      if (n.right != null) {
        code.append(n.right.val);
        remaining.add(n.right);
      }
    }
    // Make the string shorter by removing unnecessary trailing "null"s
    for (int i = code.length() - 1; i >= 0 && code.charAt(i) == ','; code.deleteCharAt(i--)) {} 
    return code.toString();
  }

  // Decodes your encoded data to tree.
  public TreeNode deserialize(String data) {
    if (data.length() == 0) return null;
    String[] nodes = data.split(",");
    TreeNode root = new TreeNode(Integer.parseInt(nodes[0]));
    Queue<TreeNode> remaining = new LinkedList<TreeNode>();
    remaining.add(root);
    int i = 1;
    while (!remaining.isEmpty() && i < nodes.length) {
      TreeNode n = remaining.remove();
      if (nodes[i].length() > 0) remaining.add(n.left = new TreeNode(Integer.parseInt(nodes[i])));
      if (i + 1 < nodes.length && nodes[i + 1].length() > 0) remaining.add(n.right = new TreeNode(Integer.parseInt(nodes[i + 1])));
      i += 2;
    }
    return root;
  }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

## Solution 2. Preorder/DFS

An iterative algorithm is more complicated to devise because we need to encode "null right tree nodes" into the serialized string in order to correctly mark the termination of a path. Conversely, a recursive solution is more straightforward and simple here.

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
    encode(root, preorder);
    return preorder.toString();
  }

  private void encode(TreeNode root, StringBuilder preorder) {
    if (root == null) {
      preorder.append(",");
      return;
    }
    preorder.append(root.val).append(",");
    encode(root.left, preorder);
    encode(root.right, preorder);
  }

  // Decodes your encoded data to tree.
  public TreeNode deserialize(String data) {
    if (data.length() == 0) return null;
    return bst(new LinkedList<String>(Arrays.asList(data.split(","))));
  }

  private TreeNode bst(Queue<String> preorder) {
    if (preorder.size() == 0) return null;
    String n = preorder.remove();
    if (n.length() == 0) return null;
    TreeNode root = new TreeNode(Integer.parseInt(n));
    root.left = bst(preorder);
    root.right = bst(preorder);
    return root;
  }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```
