# [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.

For example,

Consider the following matrix:

[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]

Given target = 3, return true.

## Solution. binary search

The matrix is just a sorted 2D array. We can apply binary search to find the target in the sorted array.

Time: O(lg(mn)) = O(lgm + lgn)

Space: O(1)

```java
public class Solution {
  public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix.length == 0 || matrix[0].length == 0) return false;
    int start = 0;
    int end = matrix.length * matrix[0].length - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2; // A technique to avoid integer overflow if `start` and `end` are large positive integers
      int r = mid / matrix[0].length;
      int c = mid % matrix[0].length;
      if (matrix[r][c] == target) return true;
      if (matrix[r][c] < target) start = mid + 1;
      else end = mid - 1;
    }
    return false;
  }
}
```
