# [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

Given a singly linked list, determine if it is a palindrome.

Follow up:
Could you do it in O(n) time and O(1) space?

## Solution 1. Reverse the entire list

A straight-forward solution is to make a copy of the original list and reverse it. Compare the reversed copy with the original list.

Time: O(n)

Space: O(n)

## Solution 2. Reverse half of the list

Similar to the integer palindrome problem, we can just reverse half of the list in-place and compare the reversed half with the other half.

Because we are not reversing the entire list, we can do the reversion in-place.

Time: O(n)

Space: O(1)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 */
public class Solution {
  public boolean isPalindrome(ListNode head) {
    ListNode x = head, y = head, z = null;
    while (y != null && y.next != null) {
      y = y.next.next;
      ListNode t = x.next;
      x.next = z;
      z = x;
      x = t;
    }
    // If the list contains odd number of nodes, x now points to the middle node.
    // Move x one step forward to make it point to the first node of the second half.
    if (y != null) x = x.next; 
    while (x != null) {
      if (x.val != z.val) return false;
      x = x.next;
      z = z.next;
    }
    return true;
  }
}
```

**How to locate the middle node of a linked list?**

1. Pointers: x = y = head
2. Loop: if y != null && y.next != null, y = y.next.next, x = x.next
3. If y != null, the list contains odd number of nodes and x now points to the middle node. Move x one step forward to make it point to the first node of the second half. Otherwise, x already points to the first node of the second half. I.e. If y != null, x = x.next

## Solution 2. Recursively compare ith and (n - i - 1)th elements

At each iteration, we reduce the list size by two.
Base case: x or xy or xx

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) { val = x; }
 */
public class Solution {
  public boolean isPalindrome(ListNode head) {
    return isPal(head, len(head)).isPal;
  }

  private Result isPal(ListNode n, int len) {
    if (len == 0) return new Result(true, null);
    if (len == 1) return new Result(true, n.next);
    if (len == 2) return new Result(n.val == n.next.val, n.next.next);
    Result r = isPal(n.next, len - 2);
    if (r.isPal) {
      r.isPal = n.val == r.node.val;
      r.node = r.node.next;
    }
    return r;
  }

  private int len(ListNode n) {
    int len = 0;
    while (n != null) {
      ++len;
      n = n.next;
    }
    return len;
  }

  private class Result {
    boolean isPal;
    ListNode node;
    Result(boolean isPal, ListNode node) {
      this.isPal = isPal;
      this.node = node;
    }
  }
}
```
