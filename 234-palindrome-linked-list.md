# [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

Given a singly linked list, determine if it is a palindrome.

## Solution 1. Reverse half of the list

A palindromic linked list looks like: aa, or aca.
We can reverse second half of the list and compare if it equals the first half.

Time: O(n)

Space: O(1) (mutate the input linked list in place)

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
    public boolean isPalindrome(ListNode head) {
		ListNode p = head;
		ListNode q = head;
		while (p != null && p.next != null) {
			p = p.next.next;
			q = q.next;
		}
		if (p != null) {
			q = q.next;
		}
		p = null;
		while (q != null) {
			ListNode tmp = q.next;
			q.next = p;
			p = q;
			q = tmp;
		}
		while (p != null) {
			if (p.val != head.val) return false;
			p = p.next;
			head = head.next;
		}
		return true;
    }
}
```

## Solution 2. Recursively compare ith and (n - i - 1)th elements

At each iteration, we reduce the list size by two.
Base case: x or xy or xx

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
  public boolean isPalindrome(ListNode head) {
    if (head == null) return true;
    return areEqual(head, len(head)).equal;
  }

  private Result areEqual(ListNode cur, int len) {
    if (len == 1) return new Result(true, cur.next);
    if (len == 2) return cur.val == cur.next.val ? new Result(true, cur.next.next) : new Result(false, null);
    Result other = areEqual(cur.next, len - 2);
    if (!other.equal) return other;
    return cur.val == other.node.val ? new Result(true, other.node.next) : new Result(false, null);
  }

  private class Result {
    boolean equal;
    ListNode node;

    Result(boolean e, ListNode n) {
      equal = e;
      node = n;
    }
  }

  private int len(ListNode head) {
    int n = 0;
    while (head != null) {
      n++;
      head = head.next;
    }
    return n;
  }
}
```
