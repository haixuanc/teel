# [48. Rotate Image](https://leetcode.com/problems/rotate-image/)

You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

Follow up:
Could you do this in-place?

## Solution 1. Rotate by circles

Time: O(n)

Space: O(1) 

```java
public class Solution {
  public void rotate(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) return;
    for (int top = 0, bottom = matrix.length - 1, left = 0, right = matrix[0].length - 1; top < bottom && left < right; top++, bottom--, left++, right--) {
      for (int offset = 0; offset < right - left; offset++) {
        int t = matrix[top][left + offset];
        matrix[top][left + offset] = matrix[bottom - offset][left];
        matrix[bottom - offset][left] = matrix[bottom][right - offset];
        matrix[bottom][right - offset] = matrix[top + offset][right];
        matrix[top + offset][right] = t;
      }
    }
  }
}
```

## Solution 2. Mathematical manipulation

**Step 1.** Transpose matrix

1 2 3        1 4 7
4 5 6   -->  2 5 8 
7 8 9        3 6 9

**Step 2.** Flip horizontally or vertically if we were rotate counterclockwise

1 4 7        7 4 1
2 5 8   -->  8 5 2
3 6 9        9 6 3

### Proof

If we were to rotate the matrix clockwise, i.e. A[i][j] -> A[j][n - 1 - i]. It can be achieved in two steps:

1. Transpose: A[i][j] -> A[j][i]
2. Horizontal flip: A[j][i] -> A[j][n - 1 - i]

```java
public class Solution {
  public void rotate(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) return;
    for (int i = 1; i < matrix.length; i++) {
      for (int j = 0; j < i; j++) {
        int t = matrix[i][j];
        matrix[i][j] = matrix[j][i];
        matrix[j][i] = t;
      }
    }
    for (int i = 0; i < matrix.length; i++) {
      for (int j = 0; j < matrix[i].length / 2; j++) {
        int t = matrix[i][j];
        matrix[i][j] = matrix[i][matrix[i].length - 1 - j];
        matrix[i][matrix[i].length - 1 - j] = t;
      }
    }
  }
}
```
