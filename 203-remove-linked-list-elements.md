# [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)

Remove all elements from a linked list of integers that have value val.

Example
Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6
Return: 1 --> 2 --> 3 --> 4 --> 5

## Solution 1. Iterative using dummy head

In linked list problems, to delete a node we need to have a reference to the node preceding the current node to be deleted, but the head node has no preceding node. To streamline programming, we use a dummy head node that points to the real head node.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode removeElements(ListNode head, int val) {
		ListNode dummyHead = new ListNode();
		dummyHead.next = head;
		// Loop invariant:
		// - Current node is not null.
		// - Value of current node does not equal to val.
		ListNode cur = dummyHead;
		while (cur.next != null) {
			if (cur.next.val == val) {
				cur.next = cur.next.next;
			} else {
				cur = cur.next;
			}
		}
		return dummyHead.next;
    }
}
```

## Solution 2. Recursive

Linked list problems are similar to binary tree problems because both data structures are recursive. Therefore, a recursive solution is usually natural to derive.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode removeElements(ListNode head, int val) {
		if (head == null) return null;
		if (head.val == val) return removeElements(head.next, val);
		head.next = removeElements(head.next, val);
		return head;
    }
}
```
