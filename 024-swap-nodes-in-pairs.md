# [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given 1->2->3->4, you should return the list as 2->1->4->3.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

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
  public ListNode swapPairs(ListNode head) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode p = dummy;
    while (p.next != null && p.next.next != null) {
      ListNode x = p.next;
      ListNode y = p.next.next;
      p.next = y;
      x.next = y.next;
      y.next = x;
      p = x;
    }
    return dummy.next;
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
  public ListNode swapPairs(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode x = head.next;
    head.next = swapPairs(head.next.next);
    x.next = head;
    return x;
  }
}
```
