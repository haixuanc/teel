# [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

Reverse a singly linked list.

Hint:

A linked list can be reversed either iteratively or recursively. Could you implement both?

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
  public ListNode reverseList(ListNode head) {
    ListNode reverse = null;
    while (head != null) {
      ListNode next = head.next;
      head.next = reverse;
      reverse = head;
      head = next;
    }
    return reverse;
  }
}
```

## Solution 2. Recursion

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
  public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode reverse = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return reverse;
  }
}
```

## Thoughts on linked list problems in general

Most linked list problems can be solved either iteratively or recursively because the data structure itself is recursive. The same applies to binary search tree problems.
