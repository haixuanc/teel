# [146. LRU Cache](https://leetcode.com/problems/lru-cache/)

Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

Follow up:
Could you do both operations in O(1) time complexity?

Example:

```
   LRUCache cache = new LRUCache( 2 /* capacity */ );

   cache.put(1, 1);
   cache.put(2, 2);
   cache.get(1);       // returns 1
   cache.put(3, 3);    // evicts key 2
   cache.get(2);       // returns -1 (not found)
   cache.put(4, 4);    // evicts key 1
   cache.get(1);       // returns -1 (not found)
   cache.get(3);       // returns 3
   cache.get(4);       // returns 4
   Show Company Tags
   Show Tags
   Show Similar Problems

```

## Solution 1. Hash table with doubly linked list running through it

[Cache replacement policies, Wikipedia](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)

Time: O(1)

Space: O(n)

```java
public class LRUCache {
  private final int capacity;
  private final Map<Integer, Node> cache;
  private Node head;
  private Node tail;

  public LRUCache(int capacity) {
    this.capacity = capacity;
    cache = new HashMap<Integer, Node>();
    head = tail = null;
  }

  public int get(int key) {
    if (!cache.containsKey(key)) return -1;
    promote(cache.get(key));
    return cache.get(key).value;
  }

  public void put(int key, int value) {
    if (cache.containsKey(key)) {
      cache.get(key).value = value;
      promote(cache.get(key));
    } else {
      cache.put(key, new Node(key, value));
      if (head == null) head = tail = cache.get(key);
      else {
        cache.get(key).next = head;
        head.prev = cache.get(key);
        head = cache.get(key);
      }
    }
    if (cache.size() > capacity) {
      cache.remove(tail.key);
      if (tail.prev != null) {
        tail.prev.next = null;
        tail = tail.prev;
      } else {
        head = tail = null;
      }
    }
  }

  private void promote(Node n) {
    if (n == head) return;
    n.prev.next = n.next;
    if (n.next != null) n.next.prev = n.prev;
    else tail = n.prev;
    n.next = head;
    head.prev = n;
    head = n;
  }

  private class Node {
    int key, value;
    Node next, prev;

    Node(int k, int v) {
      key = k;
      value = v;
      prev = next = null;
    }
  }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 *  * LRUCache obj = new LRUCache(capacity);
 *   * int param_1 = obj.get(key);
 *    * obj.put(key,value);
 *     */
 ```

## Solution 2. Use Java's built-in LinkedHashMap implementation
