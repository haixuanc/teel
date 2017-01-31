# [380. Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

Design a data structure that supports all following operations in average O(1) time.

1. insert(val): Inserts an item val to the set if not already present.
2. remove(val): Removes an item val from the set if present.
3. getRandom: Returns a random element from current set of elements. Each element must have the same probability of being returned.

Example:

```
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
```

## Solution 1. One hash table plus one list

When adding and removing a value, we need to be able to know its presence in the collection in constant time, so a data structure that uses values as index is needed, for example a set or a hash table where values are used as keys.

In order to get a random value from the collection in constant time, we first need to generate a random index and use it to fetch the value, so a data structure that uses consecutive sequential indices to reference values is needed, for example a list or a dynamic array.

```java
public class RandomizedSet {
  private final Map<Integer, Integer> indices;
  private final List<Integer> nums;
  private final java.util.Random rand;

  /** Initialize your data structure here. */
  public RandomizedSet() {
    indices = new HashMap<Integer, Integer>();
    nums = new ArrayList<Integer>(); 
    rand = new java.util.Random();
  }

  /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
  public boolean insert(int val) {
    if (indices.containsKey(val)) return false;
    indices.put(val, nums.size());
    nums.add(val);
    return true;
  }

  /** Removes a value from the set. Returns true if the set contained the specified element. */
  public boolean remove(int val) {
    if (!indices.containsKey(val)) return false;
    if (indices.get(val) < nums.size() - 1) {
      nums.set(indices.get(val), nums.get(nums.size() - 1));
      indices.put(nums.get(nums.size() - 1), indices.get(val));
    }
    indices.remove(val);
    nums.remove(nums.size() - 1);
    return true;
  }

  /** Get a random element from the set. */
  public int getRandom() {
    return nums.get(rand.nextInt(nums.size()));
  }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 *  * RandomizedSet obj = new RandomizedSet();
 *   * boolean param_1 = obj.insert(val);
 *    * boolean param_2 = obj.remove(val);
 *     * int param_3 = obj.getRandom();
 *      */
 ```

## Solution 2. Two hash tables

An array is just a special type of hash table.

```java
public class RandomizedSet {
  private final Map<Integer, Integer> indices;
  private final Map<Integer, Integer> nums;
  private final java.util.Random rand;

  /** Initialize your data structure here. */
  public RandomizedSet() {
    indices = new HashMap<Integer, Integer>();
    nums = new HashMap<Integer, Integer>(); 
    rand = new java.util.Random();
  }

  /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
  public boolean insert(int val) {
    if (indices.containsKey(val)) return false;
    indices.put(val, indices.size());
    nums.put(nums.size(), val);
    return true;
  }

  /** Removes a value from the set. Returns true if the set contained the specified element. */
  public boolean remove(int val) {
    if (!indices.containsKey(val)) return false;
    if (indices.get(val) < indices.size() - 1) {
      indices.put(nums.get(indices.size() - 1), indices.get(val));
      nums.put(indices.get(val), nums.get(indices.size() - 1));
    }
    indices.remove(val);
    nums.remove(nums.size() - 1);
    return true;
  }

  /** Get a random element from the set. */
  public int getRandom() {
    return nums.get(rand.nextInt(nums.size()));
  }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 *  * RandomizedSet obj = new RandomizedSet();
 *   * boolean param_1 = obj.insert(val);
 *    * boolean param_2 = obj.remove(val);
 *     * int param_3 = obj.getRandom();
 *      */
 ```
