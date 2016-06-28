# [90. Subsets II](https://leetcode.com/problems/subsets-ii/)

Given a collection of integers that might contain duplicates, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,2], a solution is:

[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

## Solution 1. Recursive backtrace

Time/space: O(2^n) or O(n!)

(n - 1)! + (n - 2)! + … + 1 < n x (n - 1)! = n!

```java
public class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> subsets = new ArrayList<List<Integer>>();
        dfs(subsets, new ArrayList<Integer>(), nums, 0);
        return subsets;
    }
    
    private void dfs(List<List<Integer>> subsets, List<Integer> set, int[] nums, int index) {
        subsets.add(new ArrayList<Integer>(set));
        for (int i = index; i < nums.length; ) {
            set.add(nums[i]);
            dfs(subsets, set, nums, ++i);
            set.remove(set.size() - 1);
            while (i < nums.length && nums[i] == nums[i - 1]) i++;
        }
    }
}
```

## Solution 2. Explicitly generate each subset while avoiding duplicates

- Sort the array.
- Suppose at ith iteration, we have subsets of size of s:
  - If nums[i] != nums[i - 1], we can insert nums[i] into every previous subset to generate more subsets.
  - If nums[i] == nums[i - 1], we cannot insert nums[i] into the subsets before (i - 1)th iteration; we can only insert nums[i] into every subset generated at  (i - 1)th iteration to generate more unique subsets.

```java
public class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> subsets = new ArrayList<List<Integer>>();
        subsets.add(new ArrayList<Integer>());
        int i = 0;
        for (int j = 0; j < nums.length; j++) {
            int s = subsets.size();
            for (int k = j > 0 && nums[j] == nums[j - 1] ? i : 0; k < s; k++) {
                List<Integer> set = new ArrayList<Integer>(subsets.get(k));
                set.add(nums[j]);
                subsets.add(set);
            }
            i = s;
        }
        return subsets;
    }
}
```
