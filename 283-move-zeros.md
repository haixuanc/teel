# [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.

## Solution. Two pointers

Convert the problem to moving non-zero elements to the beginning of the array.

Loop invariant:

- a[0, i): all non-zeros
- a[i, j): zeros
- a[j]: ?

Time: O(# of non-zeros)

Space: O(1)

```java
public class Solution {
    public void moveZeroes(int[] nums) {
		int i = 0;
		for (int j = 0; j < nums.length; j++) {
			if (nums[j] != 0) {
				swap(nums, i++, j);
			}
		}
    }

	private void swap(int[] nums, int i , int j) {
		if (i == j) return;
		int tmp = nums[i];
		nums[i] = nums[j];
		nums[j] = tmp;
	}
}
```
