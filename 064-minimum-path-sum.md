# [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/?tab=Description)

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

## Solution 1. DP

Time: O(mn)

Space: O(n)

```java
public class Solution {
  public int minPathSum(int[][] grid) {
    if (grid.length == 0 || grid[0].length == 0) return 0;
    int[] sums = new int[grid[0].length];
    sums[0] = grid[0][0];
    for (int i = 1; i < grid[0].length; i++) sums[i] = sums[i - 1] + grid[0][i];
    for (int i = 1; i < grid.length; i++) {
      sums[0] += grid[i][0]; 
      for (int j = 1; j < grid[i].length; j++) {
        sums[j] = Math.min(sums[j - 1], sums[j]) + grid[i][j];
      }
    }
    return sums[sums.length - 1];
  }
}
```
