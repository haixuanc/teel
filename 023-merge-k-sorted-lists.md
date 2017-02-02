# [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

## Solution 1. Divide and conquer

Time: O(lgk*n)

Space: O(lgk)

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) { val = x; }
 */
public class Solution {
  public ListNode mergeKLists(ListNode[] lists) {
    return mergeLists(lists, 0, lists.length - 1);
  }

  private ListNode mergeLists(ListNode[] lists, int start, int end) {
    if (start > end) return null;
    if (start == end) return lists[start];
    int mid = (start + end) / 2;
    return merge(mergeLists(lists, start, mid), mergeLists(lists, mid + 1, end));
  }

  private ListNode merge(ListNode l1, ListNode l2) {
    if (l1 == null) return l2;
    if (l2 == null) return l1;
    if (l1.val < l2.val) {
      l1.next = merge(l1.next, l2);
      return l1;
    } 
    l2.next = merge(l1, l2.next);
    return l2;
  }
}
```

### An iterative merge routine

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) { val = x; }
 */
public class Solution {
  public ListNode mergeKLists(ListNode[] lists) {
    return partition(lists, 0, lists.length - 1);
  }

  private ListNode partition(ListNode[] lists, int start, int end) {
    if (start > end) return null;
    if (start == end) return lists[start];
    int mid = (start + end) / 2;
    ListNode l1 = partition(lists, start, mid), l2 = partition(lists, mid + 1, end);
    ListNode dummyHead = new ListNode(0);
    ListNode p = dummyHead;
    while (l1 != null && l2 != null) {
      if (l1.val < l2.val) {
        p = p.next = l1;
        l1 = l1.next;
      } else {
        p = p.next = l2;
        l2 = l2.next;
      }
    }
    p.next = l1 != null ? l1 : l2;
    return dummyHead.next;
  }
}
```

## Solution 2. Heap

**Loop invariant:** the min node of all k list heads will be the next element.

Store the k list heads in a PriorityQueue and pop off the top one as the next node in the final sorted list. Insert the subsequent node after the top one to the PriorityQueue.

Time: O(nlgk), each node is enqueue and dequeue once. Enqueue and dequeue takes O(lgK) time.

Space: O(k)

```java
/**
 * Definition for singly-linked list.
 *  * public class ListNode {
 *   *     int val;
 *    *     ListNode next;
 *     *     ListNode(int x) { val = x; }
 */
public class Solution {
  public ListNode mergeKLists(ListNode[] lists) {
    if (lists.length == 0) return null;
    if (lists.length == 1) return lists[0];
    Queue<ListNode> min = new PriorityQueue<ListNode>(lists.length, new Comparator<ListNode>() {
        @Override
        public int compare(ListNode a, ListNode b) {
        return a.val == b.val ? 0 : (a.val < b.val ? -1 : 1);
        }
        });
    ListNode dummyHead = new ListNode(0);
    ListNode p = dummyHead;
    for (ListNode l : lists) {
      if (l != null) min.add(l);
    }
    while (min.size() > 1) {
      p = p.next = min.remove();
      if (p.next != null) min.add(p.next);
    }
    if (!min.isEmpty()) p.next = min.remove();
    return dummyHead.next;
  }
}
```
