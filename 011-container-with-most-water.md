# [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

## Solution 1. Recursive DP

Time: O(n)

Space: O(n)

```java
public class Solution {
  public int maxArea(int[] height) {
    return max(height, 0, height.length - 1);
  }

  private int max(int[] height, int left, int right) {
    if (left >= right) return 0;
    if (height[left] < height[right]) {
      return Math.max(height[left] * (right - left), max(height, left + 1, right));
    }
    return Math.max(height[right] * (right - left), max(height, left, right - 1));
  }
}
```

### A little optimization

```java
public class Solution {
  public int maxArea(int[] height) {
    return max(height, 0, height.length - 1);
  }

  private int max(int[] height, int left, int right) {
    if (left >= right) return 0;
    int min = Math.min(height[left], height[right]);
    int area = min * (right - left);
    while (left < right && height[left] <= min) left++;
    while (left < right && height[right] <= min) right--;
    return Math.max(max(height, left, right), area);
  }
}
```

## Solution 2. Naive DP

V(i, j) = the most water contained within boundary i and j

V(i, j) = 0, if i >= j

V(i, j) = max(h(i) * (j - i), v(i + 1, j)), if h(i) < h(j)

V(i, j) = max(h(j) * (j - i), v(i, j - 1)), if h(i) >= h(j)

Time: O(n^2)

Space: O(n^2)

Since the table for DP is symmetric about the diagonal (0, 0) -> (n - 1, n - 1) and cells one the diagonal are all zeros (i.e. v(i, i) = 0), we will fill the table from the cells above the diagonal and move toward the top-right corner. I.e.

1. v(0, 1), v(1, 2), v(2, 3), …
2. V(0, 2), v(1, 3), v(2, 4), …
…
n-1. V(0, n - 1)

```java
public class Solution {
  public int maxArea(int[] height) {
    if (height.length <= 1) return 0;
    int[][] areas = new int[height.length][height.length];
    int max = 0;
    for (int offset = 1; offset < areas.length; offset++) {
      for (int i = 0; i + offset < areas.length; i++) {
        areas[i][i + offset] = height[i] < height[i + offset] ? Math.max(height[i] * offset, areas[i + 1][i + offset]) : Math.max(height[i + offset] * offset, areas[i][i + offset - 1]);
        max = Math.max(max, areas[i][i + offset]);
      }
    }
    return max;
  }
}
```

## Solution 3. Two pointers

The idea is still based on the formula for the dynamic programming solutions, but we don't have to search the entire solution space.

For sub-problem of (i, j) if i < j and height(i) < height(j), the solution container either has its left boundary at i or the container is the solution for sub-problem (i + 1, j).

If i is the left boundary for the max container, the area for the container must be height(i) * (j - i). Why? Suppose the right boundary is at k, k > i && k <= j. So the height of the container is min(height(i), height(k)) <= height(i), and the width of the container k - i <= j - i. I.e. min(height(i), height(k)) * (k - i) <= height(i) * (j - i). So the max container with its left boundary at i must have an area of height(i) * (j - i).

Time: O(n), each time the two pointers get closer by at less one index until they meet

Space: O(1)

```java
public class Solution {
  public int maxArea(int[] height) {
    int max = 0;
    for (int left = 0, right = height.length - 1; left < right; ) {
      int min = Math.min(height[left], height[right]);
      max = Math.max(max, min * (right - left));
      while (left < right && height[left] <= min) left++;
      while (left < right && height[right] <= min) right--;
    }
    return max;
  }
}
```
