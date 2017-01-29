# [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

## Solution 1. Two pass copy plus hash table

A singly linked list is actually a special type of tree, an acyclic graph, so we can traverse the list safely. 

Time: O(n)

Space: O(n)

```java
/**
 * Definition for singly-linked list with a random pointer.
 *  * class RandomListNode {
 *   *     int label;
 *    *     RandomListNode next, random;
 *     *     RandomListNode(int x) { this.label = x; }
 */
public class Solution {
  public RandomListNode copyRandomList(RandomListNode head) {
    if (head == null) return null;
    Map<RandomListNode, RandomListNode> copies = new HashMap<>();
    RandomListNode p = head;
    while (p != null) {
      copies.put(p, new RandomListNode(p.label));
      p = p.next;
    }
    p = head;
    while (p != null) {
      copies.get(p).next = copies.get(p.next);
      copies.get(p).random = copies.get(p.random);
      p = p.next;
    }
    return copies.get(head);
  }
}
```
