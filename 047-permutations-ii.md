# [47. Permutations II](https://leetcode.com/problems/permutations-ii/)

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

For example,
[1,1,2] have the following unique permutations:

```
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

## Solution 1. Backtracing

Time: O(n!)

Space: O(n!)

```java
public class Solution {
  public List<List<Integer>> permuteUnique(int[] nums) {
    if (nums.length == 0) return new ArrayList<List<Integer>>();
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    Arrays.sort(nums);
    dfs(res, new ArrayList<Integer>(), nums, new boolean[nums.length]);
    return res;
  }

  private void dfs(List<List<Integer>> res, List<Integer> p, int[] nums, boolean[] used) {
    if (p.size() == nums.length) {
      res.add(new ArrayList<Integer>(p));
      return;
    }
    for (int i = 0; i < nums.length; i++) {
      if (used[i] || (i > 0 && nums[i] == nums[i - 1] && !used[i - 1])) continue;
      p.add(nums[i]);
      used[i] = true;
      dfs(res, p, nums, used);
      p.remove(p.size() - 1);
      used[i] = false;
    }
  }
}
```

## Solution 2. Backtracing - swap elements in-place

```java
public class Solution {
  public List<List<Integer>> permuteUnique(int[] nums) {
    if (nums.length == 0) return new ArrayList<List<Integer>>();
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    Arrays.sort(nums);
    dfs(res, nums, 0);
    return res;
  }

  // Generate all permutations for subarray[cur, ...]
  private void dfs(List<List<Integer>> res, int[] nums, int cur) {
    if (cur == nums.length - 1) {
      res.add(toList(nums));
      return;
    }
    for (int i = cur; i < nums.length; i++) {
      // Make the next permutation starts with a different number.
      // Since the array is already sorted, if the current number
      // is different than the previous one it does not equal any
      // previous numbers.
      if (i > cur && nums[i] == nums[cur]) continue;
      swap(nums, cur, i);
      dfs(res, nums, cur + 1);
    }
    // Restore the subarray nums[cur, ...]
    Arrays.sort(nums, cur, nums.length);
  }

  private void swap(int[] nums, int i, int j) {
    int t = nums[i];
    nums[i] = nums[j];
    nums[j] = t;
  }

  private List<Integer> toList(int[] a) {
    List<Integer> l = new ArrayList<Integer>();
    for (int i : a) l.add(i);
    return l;
  }
}
```
