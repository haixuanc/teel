# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.

click to show more practice.

More practice:
If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

## Solution 1. DP

f(i): max sum of subarray ending at ith elmement

f(0) = a[0]

f(i) = max(f(i - 1) + a[i], a[i]), if i > 0

Time: O(n)

Space: O(1)

```java
public class Solution {
  public int maxSubArray(int[] nums) {
    int max = nums[0];
    int sum = 0;
    for (int n : nums) {
      sum = Math.max(sum + n, n);
      max = Math.max(max, sum);
    }
    return max;
  }
}
```
