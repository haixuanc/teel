# [1. Two Sum](https://leetcode.com/problems/two-sum/)

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

Example:
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

UPDATE (2016/2/13):
The return format had been changed to zero-based indices. Please read the above updated description carefully.

## Solution 1. One-pass hash map

i: index of the current element
j: index of the other compliment element that satisfies `nums[i] + nums[j] = target`

Loop invariant: for every i, if j exists j is either j < i or j > i. If j < i, we should find j in the existing hash map. Otherwise, we insert i into the hashmap or replace the existing entry with the same value of nums[i]. When we encounter j at a later step, we will still be able to find a correct solution `(i, j)` .

Time: O(n)
Space: O(n)

```java
public class Solution {
  public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> seen = new HashMap<Integer, Integer>();
    for (int i = 0; i < nums.length; i++) {
      int other = target - nums[i];
      if (seen.containsKey(other)) return new int[] { seen.get(other), i };
      seen.put(nums[i], i);
    }
    return null;
  }
}
```
