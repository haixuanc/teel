# [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

Reverse a linked list from position m to n. Do it in-place and in one-pass.

For example:
Given 1->2->3->4->5->NULL, m = 2 and n = 4,

return 1->4->3->2->5->NULL.

Note:
Given m, n satisfy the following condition:
1 ≤ m ≤ n ≤ length of list.

## Solution. Two pointers

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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (m == n) return head;
        n -= m;
        ListNode pHead = new ListNode(0);
        pHead.next = head;
        ListNode a = pHead;
        while (--m > 0) {
            a = a.next;
        }
        // a -> the node before the range to be reversed
        // b -> the last node of the reversed range
        ListNode b = a.next;
        while (n-- > 0) {
            // c <- b.next.next
            // a -> b.next -> a.next
            // b -> c
            ListNode c = b.next.next;
            b.next.next = a.next;
            a.next = b.next;
            b.next = c;
        }
        return pHead.next;
    }
}
```
