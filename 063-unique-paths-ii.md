# [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/?tab=Description)

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

For example,
There is one obstacle in the middle of a 3x3 grid as illustrated below.

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```
The total number of unique paths is 2.

Note: m and n will be at most 100.

## Solution 1. DP

```java
public class Solution {
  public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    if (obstacleGrid.length == 0 || obstacleGrid[0].length == 0 || obstacleGrid[0][0] == 1) return 0;
    int[] p = new int[obstacleGrid[0].length];
    for (int i = 0; i < p.length && obstacleGrid[0][i] == 0; i++) p[i] = 1; 
    for (int i = 1; i < obstacleGrid.length; i++) {
      if (obstacleGrid[i][0] == 1) p[0] = 0;
      for (int j = 1; j < p.length; j++) {
        p[j] = obstacleGrid[i][j] == 1 ? 0 : p[j - 1] + p[j];
      }
    }
    return p[p.length - 1];
  }
}
```

A more concise version:

```java
public class Solution {
  public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    if (obstacleGrid.length == 0 || obstacleGrid[0].length == 0 || obstacleGrid[0][0] == 1) return 0;
    int[] p = new int[obstacleGrid[0].length];
    p[0] = 1;
    for (int i = 0; i < obstacleGrid.length; i++) {
      if (obstacleGrid[i][0] == 1) p[0] = 0;
      for (int j = 1; j < p.length; j++) {
        p[j] = obstacleGrid[i][j] == 1 ? 0 : p[j - 1] + p[j];
      }
    }
    return p[p.length - 1];
  }
}
```
