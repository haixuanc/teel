# [62. Unique Paths](https://leetcode.com/problems/unique-paths/?tab=Description)

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

## Solution 1. math

The robot will walk a total of (m - 1 + n - 1) steps, out of which (m - 1) will be moving down and (n - 1) will be moving right. The order for all down movements and right movements is unique.

# of all possible paths = (m + n - 2)! / ((m - 1)! * (n - 1)!)

Note: n! will easily overflow for moderate value of n so we have to use `long` instead of `int`

Time : O(m + n)

Space: O(1)

```java
public class Solution {
  public int uniquePaths(int m, int n) {
    if (m-- == 1 || n-- == 1) return 1;
    if (m < n) {
      int t = m;
      m = n;
      n = t;
    }
    long res = 1;
    for (int i = m + 1; i <= m + n; i++) res *= i;
    while (n > 1) res /= n--;
    return (int) res;
  }
}
```

## Solution 2. DP

Time: O(mn)

Space: O(n)

```java
public class Solution {
  public int uniquePaths(int m, int n) {
    if (m-- == 1 || n == 1) return 1;
    int[] p = new int[n];
    for (int i = 0; i < p.length; i++) p[i] = 1;
    while (m-- > 0) {
      for (int i = 1; i < p.length; i++) p[i] += p[i - 1];
    }
    return p[p.length - 1];
  }
}
```
