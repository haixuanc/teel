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

## Soluton 2. CTCI - Recursive compare the ith and (n-i- 1)th elements
