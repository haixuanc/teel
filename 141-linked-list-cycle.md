# [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?

## Solution 1. Slow and fast pointers

Time: O(n)

Space: O(1)

```java
/**
 * Definition for singly-linked list.
 *  * class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) {
 *      *         val = x;
 *       *         next = null;
 * }
 * /
 public class Solution {
   public boolean hasCycle(ListNode head) {
     for (ListNode p = head, q = head; p != null && p.next != null; ) {
       p = p.next.next;
       q = q.next;
       if (p == q) return true;
     }
     return false;
   }
 }
 ```
