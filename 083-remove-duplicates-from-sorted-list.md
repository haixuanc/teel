# [83. Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,
Given 1->1->2, return 1->2.
Given 1->1->2->3->3, return 1->2->3.

## Solution 1. Iterative

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
  public ListNode deleteDuplicates(ListNode head) {
    for (ListNode cur = head; cur != null && cur.next != null; ) {
      if (cur.val == cur.next.val) cur.next = cur.next.next;
      else cur = cur.next;
    }
    return head;
  }
}
```

## Solution 2. Recursive

Time: O(n)

Space: O(n)

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) { val = x; }
 */
public class Solution {
  public ListNode deleteDuplicates(ListNode head) {
    if (head == null || head.next == null) return head;
    head.next = deleteDuplicates(head.next);
    return head.val == head.next.val ? head.next : head;
  }
}
```
