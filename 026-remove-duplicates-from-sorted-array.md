# [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

## Solution 1. Two pointers

Loop invariant: all numbers in nums[.., i] are unique

At each iteration: append nums[j] to nums[..., i] only if nums[j] does not equal any number in nums[..., i]. Since nums is already sorted, nums[j] >= nums[i] > nums[k] for k in [..., i - 1]. Therefore we only need to compare nums[j] and nums[i].

Time: O(n)

Space: O(1)

```java
public class Solution {
  public int removeDuplicates(int[] nums) {
    if (nums.length <= 1) return nums.length;
    int i = 0;
    for (int j = 1; j < nums.length; ++j) {
      if (nums[j] != nums[i]) nums[++i] = nums[j];
    }
    return i + 1;
  }
}
```
