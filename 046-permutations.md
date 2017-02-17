# [46. Permutations](https://leetcode.com/problems/permutations/)

Given a collection of distinct numbers, return all possible permutations.

For example,
[1,2,3] have the following permutations:

```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

## Solution 1. Recursion

```java
public class Solution {
  public List<List<Integer>> permute(int[] nums) {
    if (nums.length == 0) return new ArrayList<List<Integer>>();
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    res.add(new ArrayList<Integer>());
    permute(res, nums, 0);
    return res;
  }

  private void permute(List<List<Integer>> res, int[] nums, int start) {
    if (start >= nums.length) return;
    permute(res, nums, start + 1);
    int last = res.size();
    while (--last >= 0) {
      List<Integer> p = res.get(last);
      for (int i = 0; i < p.size(); i++) {
        res.add(new ArrayList<Integer>(p));
        res.get(res.size() - 1).add(i, nums[start]);
      }
      p.add(nums[start]);
    }
  }
}
```

## Iteration

```java
public class Solution {
  public List<List<Integer>> permute(int[] nums) {
    if (nums.length == 0) return new ArrayList<List<Integer>>();
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    res.add(new ArrayList<Integer>());
    for (int n : nums) {
      int last = res.size();
      while (last-- > 0) {
        List<Integer> p = res.get(last);
        for (int i = 0; i < p.size(); i++) {
          res.add(new ArrayList<Integer>(p));
          res.get(res.size() - 1).add(i, n);
        }
        p.add(n);
      }
    }
    return res;
  }
}
```

## Solution 2. Backtracing

```java
public class Solution {
  public List<List<Integer>> permute(int[] nums) {
    if (nums.length == 0) return new ArrayList<List<Integer>>();
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    Set<Integer> vals = new HashSet<Integer>();
    for (int n : nums) vals.add(n);
    permute(res, new ArrayList<Integer>(), vals);
    return res;
  }

  private void permute(List<List<Integer>> res, List<Integer> p, Set<Integer> nums) {
    if (nums.isEmpty()) {
      res.add(new ArrayList<Integer>(p));
      return;
    }
    Set<Integer> vals = new HashSet<Integer>(nums);
    for (int n : nums) {
      p.add(n);
      vals.remove(n);
      permute(res, p, vals);
      p.remove(p.size() - 1);
      vals.add(n);
    }
  }
}
```
