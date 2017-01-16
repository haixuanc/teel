# [78. Subsets](https://leetcode.com/problems/subsets/)

Given a set of distinct integers, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,3], a solution is:

```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

## Solution 1. Recursion/BFS

f(0) = []

f(n) = all subsets for subarray nums[0, n]

f(n + 1) = f(n) + [s + nums[n + 1] for s in f(n)]

Time: O(2^n)

Space: O(2^n)

```java
public class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> sets = new ArrayList<List<Integer>>();
    subsets(sets, nums, 0);
    return sets;
  }

  private void subsets(List<List<Integer>> sets, int[] nums, int start) {
    if (start == nums.length) {
      sets.add(new ArrayList<Integer>());
      return;
    }
    subsets(sets, nums, start + 1);
    int last = sets.size();
    while (--last >= 0) {
      sets.add(new ArrayList<Integer>(sets.get(last)));
      sets.get(sets.size() - 1).add(nums[start]);
    }
    return;
  }
}
```

An iterative version:

```java
public class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> sets = new ArrayList<List<Integer>>();
    sets.add(new ArrayList<Integer>());
    for (int n : nums) {
      int last = sets.size();
      while (last > 0) {
        sets.add(new ArrayList<Integer>(sets.get(--last)));
        sets.get(sets.size() - 1).add(n);
      }
    }
    return sets;
  }
}
```

## Solution 2. Backtracing/DFS

All subsets can form a trie (prefix tree). The path from the root to each tree node represents a subset. E.g.

```
       0
  -----------
  |    |    |
  a    b    c
---    |
| |    c
b c
|
c
```

We visit each node of the trie in DFS order.

```java
public class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> sets = new ArrayList<List<Integer>>();
    dfs(sets, new ArrayList<Integer>(), nums, 0);
    return sets;
  }

  private void dfs(List<List<Integer>> sets, List<Integer> s, int[] nums, int start) {
    sets.add(new ArrayList<Integer>(s));
    for (int i = start; i < nums.length; i++) {
      s.add(nums[i]);
      dfs(sets, s, nums, i + 1);
      s.remove(s.size() - 1);
    }
  }
}
```

## Solution 3. Bit translation

Each subset can be encoded as a bit pattern. Iterate through all bit patterns and decode each one to a set.

Time: O(2^n)

Space: O(2^n)

```java
public class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> sets = new ArrayList<List<Integer>>();
    for (int i = 0; i < (1 << nums.length); i++) {
      sets.add(set(i, nums));
    }
    return sets;
  }

  private List<Integer> set(int mask, int[] nums) {
    List<Integer> s = new ArrayList<Integer>();
    for (int i = 0; mask > 0; i++) {
      if ((mask & 1) == 1) s.add(nums[i]);
      mask >>= 1;
    }
    return s;
  }
}
```
