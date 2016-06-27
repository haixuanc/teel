## [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

For example,
Given 1->2->3->3->4->4->5, return 1->2->5.
Given 1->1->1->2->3, return 2->3.

## Solution 1

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
    public ListNode deleteDuplicates(ListNode head) {
        ListNode pHead = new ListNode(0);
        pHead.next = head;
        ListNode prev = pHead;
        // Loop invariant:
        // 
        // for every node n in head -> ... -> prev, n.next == null or n.val != n.next.val
        // 
        // Continue while there are at least two nodes remaining in the list
        while (prev.next != null && prev.next.next != null) {
            // Include next only if next != null or next.next == null or next.val != next.next.val.
            // After the update, the loop invariant still holds.
            if (prev.next.val != prev.next.next.val) {
                prev = prev.next;
            } else {
                int v = prev.next.val;
                while (prev.next != null && prev.next.val == v) {
                    prev.next = prev.next.next;
                }
            }
        }
        return pHead.next;
    }
}
```
