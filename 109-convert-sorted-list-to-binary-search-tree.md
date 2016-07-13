## [109. Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

## Solution 1. Recursive two pointers

Time: O(nlgn)

Space: O(lgn)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
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
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) return null;
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        if (slow.next == null) return new TreeNode(slow.val);
        TreeNode root = new TreeNode(slow.next.val);
        root.right = sortedListToBST(slow.next.next);
        slow.next = null;
        root.left = sortedListToBST(head);
        return root;
    }
}
```

## Solution 2. Inorder tree traversal

The inorder sequence of a BST is in ascending order. Therefore we can go through the sorted list in order to re-construct a BST. In the meantime, we restrict that the sizes of left and right subtrees to be equal to produce a balanced BST.

Time: O(n)

Space: O(lgn)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
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
    private ListNode cur;
    
    public TreeNode sortedListToBST(ListNode head) {
        cur = head;
        int counter = 0;
        while (head != null) {
            counter++;
            head = head.next;
        }
        return inorder(1, counter);
    }
    
    // Use a couter to ensure that the list is partitioned evenly
    private TreeNode inorder(int start, int end) {
        if (start > end) return null;
        int mid = start + (end - start) / 2;
        TreeNode left = inorder(start, mid - 1);
        TreeNode root = new TreeNode(cur.val);
        cur = cur.next;
        root.left = left;
        root.right = inorder(mid + 1, end);
        return root;
    }
}
```
