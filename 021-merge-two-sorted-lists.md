# [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

## Solution 1. Iterative

Time: O(n + m)

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
  public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode sorted = new ListNode(0);
    ListNode l = sorted;
    while (l1 != null && l2 != null) {
      if (l1.val < l2.val) {
        l.next = l1;
        l1 = l1.next;
      } else {
        l.next = l2;
        l2 = l2.next;
      }
      l = l.next;
    }
    l.next = l1 != null ? l1 : l2;
    return sorted.next;
  }
}
```

## Solution. Recursive

Time: O(n + m)

Space: O(n + m) of stack frames

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) { val = x; }
 */
public class Solution {
  public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if (l1 == null) return l2;
    if (l2 == null) return l1;
    if (l1.val < l2.val) {
      l1.next = mergeTwoLists(l1.next, l2);
      return l1;
    }
    l2.next = mergeTwoLists(l1, l2.next);
    return l2;
  }
}
```

## Thought on recursive data structures

Data structures, like linked list, binary search tree, are recursive by nature. Thus problems involving the use of them can usually be solved either recursively or iteratively. Recursive solutions are usually more simple though less performant.
