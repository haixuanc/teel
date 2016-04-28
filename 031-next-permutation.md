# [31. Next Permutation](https://leetcode.com/problems/next-permutation/)

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

## Solution

- If nums is in descending order, it is the largest permuation. The next permutation is the minimum permutation which is the lexical order of characters in nums.
- Scan nums from right to left, least-significant digit to most-significant digit, find the first index i such that `nums[i] < nums[i + 1]`. Because `nums[i] < nums[i + 1]`, `nums[i:]` is not the largest permutation for segment `[i:]`. We can find the next permutation for segment `[i:]`, and the overall permutation will be the next permutation for the entire array.
  - The next permutation for segment `[i:]` is `min(nums[i+1:]) + minPermutation(nums[i+1:])`.
  - Find the smallest element in `nums[i+1:]`, say `nums[j]`, that is greater than `nums[i]`.
  - Swap `nums[i]` and `nums[j]`.
  - `nums[i+1:]` is still in descending order.
  - Reverse `nums[i+1:]`.

```java
public class Solution {
    public void nextPermutation(int[] nums) {
		int i = nums.length - 2;
		while (i >= 0 && nums[i] >= nums[i + 1]) {
			i--;
		}
		if (i < 0) {
			reverse(nums, 0, nums.length - 1);
			return;
		}
		int j = i + 1;
		// CAUTION:
		// We cannot use `nums[i] <= nums[j]`. We need to find the smallest
		// number that is greater than, not equal to, nums[i].
		//
		// nums[i + 1:] is in descending order and it is the largest permuation
		// for that segment, so nums[i:] is the largest permuation starting with
		// value nums[i] for that segment. The next permutation will be
		// min(nums[i+1:])+minPermutation(nums[i+1:]).
		while (j < nums.length && nums[i] < nums[j]) {
			j++;
		}
		j--;
		int tmp = nums[i];
		nums[i] = nums[j];
		nums[j] = tmp;
		reverse(nums, i + 1, nums.length - 1);
    }

	private void reverse(int[] nums, int start, int end) {
		for ( ; start < end; start++, end--) {
			int tmp = nums[start];
			nums[start] = nums[end];
			nums[end] = tmp;
		}
	}
}
```
