# [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note:
You are not suppose to use the library's sort function for this problem.

Follow up:
A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.

Could you come up with an one-pass algorithm using only constant space?

## Counting sort

Time: O(n + k), k is the maximum key value

Space: O(n)

Counting sort is usually used when the key values does not vary much over the input array, i.e. k is small relative to n. It is usually used as a subroutine for **radix sort**.

[Wikipedia on counting sort](https://www.wikiwand.com/en/Counting_sort)

```java
public class Solution {
    public int[] countingSort(int[] nums) {
		int[] counts = new int[3];
		for (int n : nums) counts[n]++;
		for (int i = 1; i < counts.length; i++) counts[i] += counts[i - 1];
		int[] sorted = new int[nums.length];
		// Traverse the array backward to make it a stable sort
		for (int i = nums.length - 1; i >= 0; i--) {
			sorted[--counts[nums[i]]] = nums[i];
		}
		return sorted;
    }
}
```

## Solution 1. Two-pass in-place counting sort

```java
public class Solution {
    public void sortColors(int[] nums) {
		int[] counts = new int[3];
		for (int n : nums) counts[n]++;
		for (int i = 0, j = 0; j < counts.length; ) {
			if (counts[j] == 0) {
				j++;
			} else {
				nums[i++] = j;
				counts[j]--;
			}
		}
    }
}
```

## Solution 2. Two-pointer insertion sort

Time: O(n)

Space: O(1)

```java
public class Solution {
    public void sortColors(int[] nums) {
		int lastZero = -1;
		int firstTwo = nums.length;
		// Loop invariant:
		//
		// The input looks like: [:lastZero][:][firstTwo:]
		// current index i, i > lastZero && i < firstTwo
		// elements in (lastZero: i) must be 1 or empty
		//
		// If nums[i] is 0, after swapping nums[i] with nums[lastZero + 1],
		// nums[i] must be either 1 or 0. nums[i] is in the correct position of
		// the final sorted array, so we increment i.
		//
		// If nums[i] is 2, after swapping nums[i] with nums[firstTwo - 1],
		// nums[i] may be 0, 1, or 2. nums[i] may not be in the right position
		// of the final sorted array, so we have to test it again to insert it
		// into the right position.
		for (int i = 0; i < firstTwo; ) {
			if (nums[i] == 0) {
				int tmp = nums[++lastZero];
				nums[lastZero] = nums[i];
				nums[i] = tmp;
				i++;
			} else if (nums[i] == 2) {
				int tmp = nums[--firstTwo];
				nums[firstTwo] = nums[i];
				nums[i] = tmp;
			} else {
				i++;
			}
		}
    }
}
```

## Solution 3. Generic sort k colors

Time: O(nk/2)

Space: O(1);

Iteratively put a pair of colors at the right positions. At each step, sort the current lowest and highest colors. The lowest colors will be placed at the head of the array while the highest colors will be placed at the end of the array. The array shrinks after each sort.

For each sort, we need to sort at most n elements and we will repeat sorting for k/2 times, so the time complexity is O(nk).

```java
public class Solution {
    public void sortColors(int[] nums) {
		sortKColors(nums, 3);
	}

	private void sortKColors(int[] nums, int k) {
		int low = 0;
		int high = k - 1;
		int lastLow = -1;
		int firstHigh = nums.length;
		while (low < high) {
			for (int i = lastLow + 1; i < firstHigh; ) {
				if (nums[i] == low) {
					int tmp = nums[++lastLow];
					nums[lastLow] = nums[i];
					nums[i] = tmp;
					i++;
				} else if (nums[i] == high) {
					int tmp = nums[--firstHigh];
					nums[firstHigh] = nums[i];
					nums[i] = tmp;
				} else {
					i++;
				}
			}
			low++;
			high--;
		}
	}
}
```
