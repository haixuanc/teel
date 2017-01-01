# [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

Given a linked list, remove the nth node from the end of list and return its head.

For example,

    Given linked list: 1->2->3->4->5, and n = 2.

    After removing the second node from the end, the linked list becomes 1->2->3->5.

Note:

Given n will always be valid.
Try to do this in one pass.

## Solution 1

A straight-forward solution is to traverse the list first to determine its length. Then we will be able to know the position of the nth-but-last node from the beginning of the list.

## Solution 1. Two pointers

Pointer x: points to the node before the node to be removed
Pointer y: points to the last node of the list

Time: O(n)

Space: O(1)

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) { val = x; }
 */
public class Solution {
  public ListNode removeNthFromEnd(ListNode head, int n) {
    if (n <= 0) return head;
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode x = dummy;
    while (n-- > 0 && x != null) {
      x = x.next;
    }
    if (x == null) return head; // Illegal value for n
    ListNode y = dummy;
    while (x.next != null) {
      x = x.next;
      y = y.next;
    }
    y.next = y.next.next;
    return dummy.next;
  }
}
```
