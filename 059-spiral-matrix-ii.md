# [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)

Given an integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.

For example,
Given n = 3,

You should return the following matrix:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]

## Solution

Loop invariant:

Traverse the matrix in spiral order as long as numbers of rows and columns are positive.

```
public class Solution {
  public int[][] generateMatrix(int n) {
    int[][] matrix = new int[n][n];
    for (int left = -1, right = n - 1, top = 0, bottom = n - 1, v = 1; left < right; ) {
      for (int i = ++left; i <= right; i++) matrix[top][i] = v++;
      for (int i = ++top; i <= bottom; i++) matrix[i][right] = v++;
      for (int i = --right; i >= left; i--) matrix[bottom][i] = v++;
      for (int i = --bottom; i >= top; i--) matrix[i][left] = v++;
    }
    return matrix;
  }
}
```

```java
public class Solution {
    public int[][] generateMatrix(int n) {
		int[][] matrix = new int[n][n];
		int row = 0, col = -1, m = n, count = 0;
		// A useful technique:
		//
		// Use prefix self-increment operator rather than suffix to avoid array
		// index out-of-bound error.
		while (true) {
			for (int i = 0; i < n; i++) matrix[row][++col] = ++count;
			if (--m <= 0) break; // Edge case: n = 0
			for (int i = 0; i < m; i++) matrix[++row][col] = ++count;
			if (--n == 0) break;
			for (int i = 0; i < n; i++) matrix[row][--col] = ++count;
			if (--m == 0) break;
			for (int i = 0; i < m; i++) matrix[--row][col] = ++count;
			if (--n == 0) break;
		}
		return matrix;
    }
}
```

## Advantage of prefix self-increment operator over suffix self-increment operator

Usually the loop invariant only guarantees the current index is valid. If `i = size - 1`, `i++` will be evaluated as `size` in the next iteration, and it will cause array index out-of-bound error.
If we use `++i` instead, the value of `i` won't change after current iteration. The loop invariant will still hold at the beginning of the next iteration. Therefore, we don't need to worry about array index out-of-bound errors.
