# [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

## Solution 1. Binary search

Time: O(lgn)

Space: O(1)

```java
public class Solution {
  public int search(int[] nums, int target) {
    for (int left = 0, right = nums.length - 1; left <= right; ) {
      int mid = (left + right) / 2;
      if (nums[mid] == target) return mid;
      if ((nums[left] <= target && target < nums[mid]) || (nums[mid] <= nums[right] && (target < nums[mid] || target > nums[right]))) right = mid - 1;
      else left = mid + 1;
    }
    return -1;
  }
}
```

## Solution 2. Mutate array on the fly to make it an ascending sequence

[Change the array into an ascending sequence on the fly so we can apply a normal binary search.](https://discuss.leetcode.com/topic/34491/clever-idea-making-it-simple)

Time: O(lgn)

Space: O(1)

```java
public class Solution {
  public int search(int[] nums, int target) {
    for (int left = 0, right = nums.length - 1; left <= right; ) {
      int mid = (left + right) / 2;
      int val = nums[mid] >= nums[left] ? (target >= nums[left] ? nums[mid] : Integer.MIN_VALUE) : (target >= nums[left] ? Integer.MAX_VALUE : nums[mid]);
      if (target == val) return mid;
      if (target < val) right = mid - 1;
      else left = mid + 1;
    }
    return -1;
  }
}
```
