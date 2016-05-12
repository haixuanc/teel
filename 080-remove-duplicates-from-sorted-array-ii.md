# [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

For example,
Given sorted array nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3. It doesn't matter what you leave beyond the new length.

## Solution. Two pointers

Loop invariant:

- [:i]: resulting array that has no more than two duplicates
- j: current index
- Only increment i if
  - nums[j] != nums[i]
  - or nums[j] == nums[i] && nums[i] != nums[i - 1]

Time: O(n)

Space: O(1), we don't need to memorize all occuring elements because the array is sorted. If nums[i] != nums[i - 1], then nums[i] != nums[j] for j in [0, i - 1).

```java
public class Solution {
    public int removeDuplicates(int[] nums) {
		if (nums.length <= 2) return nums.length;
		int i = 0;
		for (int j = 1, count = 1; j < nums.length; j++) {
			if (nums[j] == nums[i] && count < 2) {
				nums[++i] = nums[j];
				count++;
			} else if (nums[j] != nums[i]) {
				nums[++i] = nums[j];
				count = 1;
			}
		}
		return i + 1;
    }
}
```

## General thought on two-pointer solution for sequential data structures

The question is usually finding a subsequence satisfying certain criteria or modifying a sequence in place. The general idea is to use one pointer to reference the end of the subsequence and another pointer to reference the current element under examination. Before each iteration, the subsequence of [start:i] is valid. We only increment i if element at index j is valid, so the subsequence remains valid after the iteration.
