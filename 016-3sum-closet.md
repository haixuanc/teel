# [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/)

Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

    For example, given array S = {-1 2 1 -4}, and target = 1.

        The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

## Solution 1. Two pointers

This problem is similar to the 3sum problem. The difference is to determine how to move the two inner pointers.

On the other hand, the hash table idea is not applicable here because we are not looking for an exact number when comparing the sum with the target.

Time: O(n^2)

Space: O(1)

```java
public class Solution {
  public int threeSumClosest(int[] nums, int target) {
    Arrays.sort(nums);
    int minDiff = Integer.MAX_VALUE;
    for (int i = 0; i <= nums.length - 3; i++) {
      for (int j = i + 1, k = nums.length - 1; j < k; ) {
        int diff = target - nums[i] - nums[j] - nums[k];
        if (diff == 0) return target;
        if (Math.abs(diff) < Math.abs(minDiff)) minDiff = diff;
        if (diff < 0) k--;
        else j++;
      }
    }
    return target - minDiff;
  }
}
```
