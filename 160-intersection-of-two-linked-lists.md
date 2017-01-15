# [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

Write a program to find the node at which the intersection of two singly linked lists begins.


For example, the following two linked lists:

```
A:          a1 → a2
                    ↘
                    c1 → c2 → c3
                    ↗            
B:     b1 → b2 → b3
```

begin to intersect at node c1.

Notes:

If the two linked lists have no intersection at all, return null.
The linked lists must retain their original structure after the function returns.
You may assume there are no cycles anywhere in the entire linked structure.
Your code should preferably run in O(n) time and use only O(1) memory.

## Solution 1. Iterative two pointers

Time: O(n)

Space: O(1)

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) {
 *      *         val = x;
 *       *         next = null;
 * }
 * */
public class Solution {
  public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode p = headA, q = headB;
    while (p != null && q != null) {
      if (p == q) return p;
      p = p.next;
      q = q.next;
      if (p == null && q == null) return null;
      if (p == null) p= headB;
      if (q == null) q = headA;
    }
    return null;
  }
}
```

A more concise version:

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) {
 *      *         val = x;
 *       *         next = null;
 * }
 **/
public class Solution {
  public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode p = headA, q = headB;
    while (p != q) {
      p = p == null ? headB : p.next;
      q = q == null ? headA: q.next;
    }
    return p;
  }
}
```
