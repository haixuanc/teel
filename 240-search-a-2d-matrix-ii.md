# [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted in ascending from left to right.
Integers in each column are sorted in ascending from top to bottom.
For example,

Consider the following matrix:

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = 5, return true.

Given target = 20, return false.

## Solution 1. Cancel a row or column at a time

Time: O(m+n)

Space: O(1)

```java
public class Solution {
  public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix.length == 0 || matrix[0].length == 0) return false;
    for (int top = 0, right = matrix[0].length - 1; top < matrix.length && right >= 0; ) {
      if (matrix[top][right] == target) return true;
      if (matrix[top][right] < target) top++;
      else right--;
    }
    return false;
  }
}
```

## Solution 2. Quad-search

We divide the matrix into four parts by its center. Each time, we compare the center with the target and we can exclude one sub if center does not equal to the target.

Time: O(lgm + lgn)

Space: O(max(lgm, lgn)), number of stack frames

```java
public class Solution {
  public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix.length == 0 || matrix[0].length == 0) return false;
    return search(matrix, 0, matrix.length - 1, 0, matrix[0].length - 1, target);
  }

  private boolean search(int[][] matrix, int top, int bottom, int left, int right, int target) {
    if (top < 0 || top > bottom || bottom >= matrix.length || left < 0 || left > right || right >= matrix[0].length) return false;
    int row = top + (bottom - top) / 2;
    int col = left + (right - left) / 2;
    if (matrix[row][col] == target) return true;
    if (matrix[row][col] < target) return search(matrix, top, row, col + 1, right, target) || search(matrix, row + 1, bottom, left, right, target);
    return search(matrix, top, row - 1, left, right, target) || search(matrix, row, bottom, left, col - 1, target);
  }
}
```
