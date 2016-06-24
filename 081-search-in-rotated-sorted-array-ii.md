# [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

Follow up for "Search in Rotated Sorted Array":
What if duplicates are allowed?

Would this affect the run-time complexity? How and why?

Write a function to determine if a given target is in the array.

## Solution

If target != a[mid] and a[left] = a[right] = a[mid], target might fall into the left or right part. Therefore the worst-case run time is O(n).

Best-case run time: O(lgn)

Worst-cast run time: O(n)

```java
public class Solution {
    public boolean search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return true;
            if (nums[mid] < nums[right]) {
                if (target > nums[mid] && target <= nums[right]) left = mid + 1;
                else right = mid - 1;
            } else if (nums[mid] > nums[right]) {
                if (target >= nums[left] && target < nums[mid]) right = mid - 1;
                else left = mid + 1;
            } else {
                // A small optimization:
                // Worst-cast run time is O(n/2)
                if (nums[mid] == nums[left]) {
                    left++;
                    right--;
                } else right = mid - 1;
            }
        }
        return false;
    }
}
```
