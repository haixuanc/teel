# [55. Jump Game](https://leetcode.com/problems/jump-game/?tab=Description)

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

For example:
A = [2,3,1,1,4], return true.

A = [3,2,1,0,4], return false.

## Solution 1. Naive DP

Time: O(n^2)

Space: O(n)

```java
public class Solution {
  public boolean canJump(int[] nums) {
    if (nums.length == 0) return false;
    boolean[] isReachable = new boolean[nums.length];
    isReachable[nums.length - 1] = true;
    for (int i = nums.length - 2; i >= 0; i--) {
      for (int j = i + 1; j < nums.length && j <= i + nums[i]; j++) {
        if (isReachable[j]) {
          isReachable[i] = true;
          break;
        }
      }
    }
    return isReachable[0];
  }
}
```

## Solution 2. Greedy

Time: O(n)

Space: O(1)

```java
public class Solution {
  public boolean canJump(int[] nums) {
    int max = 0;
    for (int i = 0; max < nums.length - 1 && i <= max; i++) {
      max = Math.max(max, i + nums[i]);
    }
    return max >= nums.length - 1;
  }
}
```
