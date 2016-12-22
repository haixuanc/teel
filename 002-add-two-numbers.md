# [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

## Thought on iterative programs in general

This question represents a large group of problems occurring in technical interviews or daily computing scenarios. The program executes in a repeated manner and terminates only when a certain condition is met. Therefore we can derive a general algorithm for this kind of problems:

- Define a loop and its **termination condition**
- Define a loop invariant

These two elements will help make the main body of your program and verify its correctness.

## Solution 1

Continue the program as long as `l1` or `l2` is not exhausted or `carry` is true. The reverse will make the **termination condition** for this program.

Time: O(max(m, n))

Space: O(max(m, n))

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) { val = x; }
 */
public class Solution {
  public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    if (l1 == null) return l2;
    if (l2 == null) return l1;
    ListNode s = new ListNode(l1.val + l2.val);
    // Loop invariant:
    // - cur = last (highest) digit in the resulting number
    // - l1 = last (highest) digit in l1
    // - l2 = last (highest) digit in l2
    // - carry = if current addition results in carry-one to the next digit
    // 
    // Continue the loop as long as:
    // - l1 or l2 is not exhausted
    // - or carry is true
    boolean carry = s.val > 9;
    if (carry) s.val -= 10;
    l1 = l1.next;
    l2 = l2.next;
    ListNode cur = s;
    while (l1 != null && l2 != null) {
      cur.next = new ListNode(l1.val + l2.val + (carry ? 1 : 0));
      carry = cur.next.val > 9;
      if (carry) cur.next.val -= 10;
      cur = cur.next;
      l1 = l1.next;
      l2 = l2.next;
    }
    l1 = l1 != null ? l1 : l2;
    while (l1 != null) {
      cur.next = new ListNode(l1.val + (carry ? 1 : 0));
      carry = cur.next.val > 9;
      if (carry) cur.next.val -= 10;
      cur = cur.next;
      l1 = l1.next;
    }
    if (carry) cur.next = new ListNode(1);
    return s;
  }
}
```

## Solution 2. A samll optimization

If one list is exhausted, we can simply append the remaining part of the other list to the end of the resulting list. We can stop iterating the remaining digits once there is no carry-one because we don't have to modify any of the remaining digits.

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) { val = x; }
 */
public class Solution {
  public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    if (l1 == null) return l2;
    if (l2 == null) return l1;
    ListNode s = new ListNode(l1.val + l2.val);
    // Loop invariant:
    // - cur = last (highest) digit in the resulting number
    // - l1 = last (highest) digit in l1
    // - l2 = last (highest) digit in l2
    // - carry = if current addition results in carry-one to the next digit
    // 
    // Continue the loop as long as:
    // - l1 or l2 is not exhausted
    // - or carry is true
    boolean carry = s.val > 9;
    if (carry) s.val -= 10;
    l1 = l1.next;
    l2 = l2.next;
    ListNode cur = s;
    while (l1 != null && l2 != null) {
      cur.next = new ListNode(l1.val + l2.val + (carry ? 1 : 0));
      carry = cur.next.val > 9;
      if (carry) cur.next.val -= 10;
      cur = cur.next;
      l1 = l1.next;
      l2 = l2.next;
    }
    cur.next = l1 != null ? l1 : l2;
    while (carry && cur.next != null) {
      cur = cur.next;
      cur.val += 1;
      carry = cur.val > 9;
      if (carry) cur.val -= 10;
    }
    if (carry) cur.next = new ListNode(1);
    return s;
  }
}
```
