34. Search for a Range   My Submissions QuestionEditorial Solution

Given a sorted array of integers, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

For example,
Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4].




public class Solution {
    public int[] searchRange(int[] nums, int target) {
		return range(nums, target, 0, nums.length - 1);
    }

	private int[] range(int[] nums, int target, int start, int end) {
		int[] r = { -1, -1 };
		if (start > end) return r;
		int mid = (start + end) / 2;
		if (nums[mid] == target) {
			r[0] = search(nums, target, start, mid, true);
			r[1] = search(nums, target, mid, end, false);
			return r;
		}
		if (nums[mid] < target) return range(nums, target, mid + 1, end);
		return range(nums, target, start, mid - 1);
	}

	// Similar to problem `278. first bad version`
	// Lower bound: first bad version
	// Upper bound: last bad version
	// Input looks like: x..tt..t..y
	private int search(int[] nums, int target, int start, int end, boolean lowerBound) {
		if (start > end) return -1;
		while (start < end) {
			int mid = (start + end) / 2;
			if (nums[mid] == target) {
				if (lowerBound) end = mid;
				else start = mid;
			} else if (nums[mid] < target) {
				start = mid + 1;
			} else {
				end = mid - 1;
			}
		}
		// It is not guaranteed that target is in the array
		return nums[start] == target ? start : -1;
	}
}

## Toughts on binary search

When thinking about how binary search works, it is better to treat the problem as removing unwanted elements from the array until the only element left is target so target is found, or array becomes empty therefore target is not found.

The problem becomes more tedious if we try to search for target in possible range. When we deal with possible range that target may reside in, we have to handle edge cases properly. E.g. range is empty, or size of range is one, or target is not in the range.

The problem becomes much easier and cleaner if we solve it in an opposite way. At each step, we remove unwanted elements from the array. We repeat this step until array becomes empty or the only left element is the target.

The **loop invariant** of binary search and other search algorithms:

Target may exist in the remaining elements. Taget does not exist in the removed elements.

At each step we remove unwanted elements based on some filtering criteria from the collection to maintain the loop invariant and reduce the size of the problem until solution is found.
