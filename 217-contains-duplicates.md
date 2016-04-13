# [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

## Solution 1. Quick sort based

If k is the current index, divide the array into four segments:

[0, i), [i, j], (j, k), [k, n - 1]

- [0, i): elements < pivot
- [i, j]: elements = pivot
- (j, k): elements > pivot
- [k, n - 1]: elements ? pivot

If j - i >= 1, array contains duplicates.

```java
public class Solution {
    public boolean containsDuplicate(int[] nums) {
		return quickSort(nums, 0, nums.length - 1);
    }

	private boolean quickSort(int[] nums, int start, int end) {
		if (start >= end) return false;
		int tmp = nums[start];
		nums[start] = nums[(start + end) / 2];
		nums[(start + end) / 2] = tmp;
		int i = start;
		int j = start;
		for (int k = start + 1; k <= end; k++) {
			if (nums[k] < nums[i]) {
				int cur = nums[k];
				nums[k] = nums[++j];
				nums[j] = nums[i++];
				nums[i - 1] = cur;
			} else if (nums[k] == nums[i]) {
				int cur = nums[k];
				nums[k] = nums[++j];
				nums[j] = cur;
			}
		}
		if (j - i >= 1) return true;
		return quickSort(nums, start, i - 1) || quickSort(nums, j + 1, end);
	}
}
```

## Soluiton 2. Double-for-loop

## Solution 3. Hash set

## Solution 4. Sort and search
