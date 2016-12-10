# [167. Two Sum II - Input array is sorted   Add to List QuestionEditorial Solution](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2

## Solution 1. Two pointers

Suppose a solution of ordered pair (i, j) exists, whereas 0 <= i < j <= n - 1.

Initialization: i = 0, j = n -1

Loop invariant: at every step, if `nums[i] + nums[j] = target`, we find a solution. If `nums[i] + nums[j] < target`, we should increase `nums[i]` or `nums[j]` to make the sum closer to the target. Since `nums[j]` is already the greatest element in the remaining range, we are only able to increase `nums[i]`. Therefore we update `i` to `i+1`. The same applies to the condition where `nums[i] + nums[j] > target`.

Time: O(n)
Space: O(1)

```java
public class Solution {
  public int[] twoSum(int[] numbers, int target) {
    for (int i = 0, j = numbers.length - 1; i < j; ) {
      if (numbers[i] + numbers[j] == target) return new int[] { i + 1, j + 1 };
      if (numbers[i] + numbers[j] < target) ++i;
      else --j;
    }
    return null;
  }
}
```

## Solution 2. Binary search to find the compliment element

Time: O(nlgn)
Space: O(1)

```java
public class Solution {
  public int[] twoSum(int[] numbers, int target) {
    for (int i = 0; i < numbers.length - 1; i++) {
      int j = i + 1;
      int k = numbers.length - 1;
      int other = target - numbers[i];
      while (j <= k && numbers[j] <= other && numbers[k] >= other) {
        int mid = j + (k - j) / 2;
        if (numbers[mid] == other) return new int[] { i + 1, mid + 1 };
        if (numbers[mid] < other) j = mid + 1;
        else k = mid - 1;
      }
    }
    return null;
  }
}
```
