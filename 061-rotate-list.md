# [61. Rotate List](https://leetcode.com/problems/rotate-list/?tab=Description)

Given a list, rotate the list to the right by k places, where k is non-negative.

For example:
Given 1->2->3->4->5->NULL and k = 2,
return 4->5->1->2->3->NULL.

## Solution

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
  public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null || k == 0) return head;
    int size = 1;
    ListNode tail = head;
    while (tail.next != null) {
      size++;
      tail = tail.next;
    }
    k %= size;
    if (k == 0) return head;
    k = size - k;
    ListNode n = head;
    while (--k > 0) {
      n = n.next;
    }
    tail.next = head;
    head = n.next;
    n.next = null;
    return head;
  }
}
```
