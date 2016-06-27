# [86. Partition List](https://leetcode.com/problems/partition-list/)

Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

For example,
Given 1->4->3->2->5->2 and x = 3,
return 1->2->2->4->3->5.

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
    public ListNode partition(ListNode head, int x) {
        ListNode smaller = new ListNode(0);
        ListNode bigger = new ListNode(0);
        ListNode s = smaller;
        ListNode b = bigger;
        while (head != null) {
            if (head.val < x) {
                s.next = head;
                s = s.next;
            } else {
                b.next = head;
                b = b.next;
            }
            head = head.next;
        }
        s.next = bigger.next;
        b.next = null;
        return smaller.next;
    }
}
```
