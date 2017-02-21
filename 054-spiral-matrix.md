# [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/?tab=Description)

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

For example,
Given the following matrix:

```
[
  [ 1, 2, 3 ],
  [ 4, 5, 6 ],
  [ 7, 8, 9 ]
]
```

You should return [1,2,3,6,9,8,7,4,5].

## Solution

Time: O(mn)

Space: O(mn)

```java
public class Solution {
  public List<Integer> spiralOrder(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) return new ArrayList<Integer>();
    List<Integer> order = new ArrayList<Integer>();
    for (int top = 0, bottom = matrix.length - 1, left = 0, right = matrix[0].length - 1; top <= bottom && left <= right; top++, bottom--, left++, right--) {
      if (top == bottom) {
        for (int i = left; i <= right; i++) order.add(matrix[top][i]);
        break;
      }
      if (left == right) {
        for (int i = top; i <= bottom; i++) order.add(matrix[i][left]);
        break;
      }
      for (int i = left; i < right; i++) order.add(matrix[top][i]);
      for (int i = top; i < bottom; i++) order.add(matrix[i][right]);
      for (int i = right; i > left; i--) order.add(matrix[bottom][i]);
      for (int i = bottom; i > top; i--) order.add(matrix[i][left]);
    }
    return order;
  }
}
```
