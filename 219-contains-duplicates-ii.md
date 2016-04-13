# [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.

## Solution. Sliding window

The problem can be converted to the problem of finding duplicates in a sub-array of length of k + 1.

```java
public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
		if (k <= 0) return false;
		// CAUTION:
		// No need to use a hash map to keep track of indices of 
		// unique elements, because all elements seen so far in
		// the current window are unqiue. Otherwise, we find 
		// duplicates whithin the range.
		Set<Integer> uniques = new HashSet<Integer>();
		for (int i = 0; i < nums.length; i++) {
			if (i > k) {
				uniques.remove(nums[i - k - 1]);
			}
			if (uniques.contains(nums[i])) return true;
			uniques.add(nums[i]);
		}
		return false;
    }
}
```
