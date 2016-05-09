# [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.

Follow up:
- Did you use extra space?
- A straight forward solution using O(mn) space is probably a bad idea.
- A simple improvement uses O(m + n) space, but still not the best solution.
- Could you devise a constant space solution?

## Solution 1. Use two extra arrays

We can use two extra arrays to store the result of rows and columns.

Time: O(mn)

Space: O(m + n)

## Solution 2. Constant space

Store the results in the first row and column. Note matrix[0][0] can only represent the first row or the first column, it cannot represent both. We have to use one extra variable.

Time: O(mn)

Space: O(1)

```java
public class Solution {
  public void setZeroes(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) return;
    boolean col0 = false;
    // NOTE:
    // If matrix[0][0] is zero, it represents both first row and first column are all zeros.
    // But a cell can only represent a row or a column, it cannot represent both.
    // Therefore we use another variable `col0` to represent the result of the first column.
    //
    // matrix[0:][0] represents the results of rows[0:]
    // matrix[0][1:] represents the results of columns[1:]
    // col0 represents the result of column[0]
    for (int row = 0; row < matrix.length; row++) {
      if (matrix[row][0] == 0) col0 = true;
      for (int col = 1; col < matrix[0].length; col++) {
        if (matrix[row][col] == 0) matrix[0][col] = matrix[row][0] = 0;
      }
    }
    // NOTE:
    // We cannot modify matrix[0][col] and matrix[row][0] while updating matrix[row][col]
    for (int row = matrix.length - 1; row >= 0; row--) {
      for (int col = matrix[0].length - 1; col > 0; col--) {
        if (matrix[0][col] == 0 || matrix[row][0] == 0) matrix[row][col] = 0;
      }
      if (col0) matrix[row][0] = 0;
    }
  }
}
```
