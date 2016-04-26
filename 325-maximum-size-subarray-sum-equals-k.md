# [325. Maximum Size Subarray Sum Equals k](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/)

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

Example 1:
Given nums = [1, -1, 5, -2, 3], k = 3,
return 4. (because the subarray [1, -1, 5, -2] sums to 3 and is the longest)

Example 2:
Given nums = [-2, -1, 2, 1], k = 1,
return 2. (because the subarray [-1, 2] sums to 1 and is the longest)

Follow Up:
Can you do it in O(n) time?

## Solution. Hash table

It is a modification of the two-sum problem. If we convert the array into an array of accumulated sums, then it changes to a problem of finding max(j - i) where sum[j] - sum[i] = k.

We can also use the double for-loop solution. But we cannot use the binary search solution because we need to keep elements' position unchanged in order to calculate accumulated sums and range widths correctly.

```java
public class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
		Map<Integer, Integer> sums = new HashMap<Integer, Integer>();
		sums.put(0, -1);
		int max = 0;
		int total = 0;
		for (int i = 0; i < nums.length; i++) {
			total += nums[i];
			if (sums.containsKey(total - k)) {
				max = Math.max(max, i - sums.get(total - k));
			}
			// If there are duplicates, keep the left-most one.
			if (!sums.containsKey(total)) {
				sums.put(total, i);
			}
		}
		return max;
    }
}
```
