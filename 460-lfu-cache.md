# [460. LFU Cache](https://leetcode.com/problems/lfu-cache/)

Design and implement a data structure for Least Frequently Used (LFU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reaches its capacity, it should invalidate the least frequently used item before inserting a new item. For the purpose of this problem, when there is a tie (i.e., two or more keys that have the same frequency), the least recently used key would be evicted.

Follow up:
Could you do both operations in O(1) time complexity?

Example:

```
   LFUCache cache = new LFUCache( 2 /* capacity */ );

   cache.put(1, 1);
   cache.put(2, 2);
   cache.get(1);       // returns 1
   cache.put(3, 3);    // evicts key 2
   cache.get(2);       // returns -1 (not found)
   cache.get(3);       // returns 3.
   cache.put(4, 4);    // evicts key 1.
   cache.get(1);       // returns -1 (not found)
   cache.get(3);       // returns 3
   cache.get(4);       // returns 4
   Show Company Tags
   Show Tags
   Show Similar Problems
```

## Solution 1

```java
public class LFUCache {
  private final Map<Integer, Integer> values;
  private final Map<Integer, Integer> freqs;
  private final Map<Integer, Set<Integer>> groups;
  private int min;
  private final int capacity;

  public LFUCache(int capacity) {
    this.capacity = capacity;
    values = new HashMap<Integer, Integer>();
    freqs = new HashMap<Integer, Integer>();
    groups = new HashMap<Integer, Set<Integer>>();
    min = 0;
    groups.put(0, new LinkedHashSet<Integer>());
  }

  public int get(int key) {
    if (!values.containsKey(key)) return -1;
    incFreq(key);
    return values.get(key);
  }

  public void put(int key, int value) {
    if (capacity == 0) return;
    // NOTE: we need to remove the oldest least frequently used element
    // before inserting a new element, because if the least frequency is
    // greater than one the newly inserted element will be erased
    // immediately otherwise.
    if (values.size() == capacity && !values.containsKey(key)) {
      int oldest = -1;
      for (int k : groups.get(min)) {
        oldest = k;
        break;
      }
      groups.get(min).remove(oldest);
      values.remove(oldest);
      freqs.remove(oldest);
    }
    values.put(key, value);
    incFreq(key);
  }

  private void incFreq(int key) {
    freqs.put(key, freqs.containsKey(key) ? freqs.get(key) + 1 : 1);
    if (groups.containsKey(freqs.get(key) - 1)) groups.get(freqs.get(key) - 1).remove(key);
    if (!groups.containsKey(freqs.get(key))) groups.put(freqs.get(key), new LinkedHashSet<Integer>());
    groups.get(freqs.get(key)).add(key);
    if (freqs.get(key) < min) min = freqs.get(key);
    else if (freqs.get(key) - 1 == min && groups.get(min).isEmpty()) min++;
  }
}

/**
 * Your LFUCache object will be instantiated and called as such:
 *  * LFUCache obj = new LFUCache(capacity);
 *   * int param_1 = obj.get(key);
 *    * obj.put(key,value);
 *     */
 ```
