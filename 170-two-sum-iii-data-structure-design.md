# [170. Two Sum III - Data structure design](https://leetcode.com/problems/two-sum-iii-data-structure-design/)

Design and implement a TwoSum class. It should support the following operations: add and find.

add - Add the number to an internal data structure.
find - Find if there exists any pair of numbers which sum is equal to the value.

For example,
add(1); add(3); add(5);
find(4) -> true
find(7) -> false

## Solution 1. More frequent calls to add()

- `add()`: O(1)
- `find()`: O(n)
- space cost: O(n)

```java
public class TwoSum {
  private Map<Integer, Integer> freq = new HashMap<Integer, Integer>();

  // Add the number to an internal data structure.
  public void add(int number) {
    freq.put(number, freq.containsKey(number) ? freq.get(number) + 1 : 1);
  }

  // Find if there exists any pair of numbers which sum is equal to the value.
  public boolean find(int value) {
    for (int n : freq.keySet()) {
      int other = value - n;
      if (other == n ? freq.get(n) >= 2 : freq.containsKey(other)) return true;
    }
    return false;
  }
}


// Your TwoSum object will be instantiated and called as such:
// TwoSum twoSum = new TwoSum();
// twoSum.add(number);
// twoSum.find(value);
```

## Solution 2. More frequent calls to find()

- `add()`: O(n)
- `find()`: O(1)
- Space cost: O(n)

```java
public class TwoSum {
  private Set<Integer> nums = new HashSet<Integer>();
  private Set<Integer> sums = new HashSet<Integer>();

  // Add the number to an internal data structure.
  public void add(int number) {
    if (nums.contains(number)) {
      sums.add(number << 1);
      return;
    }
    for (int n : nums) sums.add(n + number);
    nums.add(number);
  }

  // Find if there exists any pair of numbers which sum is equal to the value.
  public boolean find(int value) {
    return sums.contains(value);
  }
}


// Your TwoSum object will be instantiated and called as such:
// TwoSum twoSum = new TwoSum();
// twoSum.add(number);
// twoSum.find(value);
```
