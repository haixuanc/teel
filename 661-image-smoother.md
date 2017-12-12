# [661. Image Smoother](https://leetcode.com/problems/image-smoother/description/)

Given a 2D integer matrix M representing the gray scale of an image, you need to design a smoother to make the gray scale of each cell becomes the average gray scale (rounding down) of all the 8 surrounding cells and itself. If a cell has less than 8 surrounding cells, then use as many as you can.

Example 1:

```
Input:
[[1,1,1],
[1,0,1],
[1,1,1]]

Output:
[[0, 0, 0],
[0, 0, 0],
[0, 0, 0]]

Explanation:
For the point (0,0), (0,2), (2,0), (2,2): floor(3/4) = floor(0.75) = 0
For the point (0,1), (1,0), (1,2), (2,1): floor(5/6) = floor(0.83333333) = 0
For the point (1,1): floor(8/9) = floor(0.88888889) = 0

Note:
The value in the given matrix is in the range of [0, 255].
The length and width of the given matrix are in the range of [1, 150].
```

## Solution 1

Time: O(mn)

Space: O(mn)

```java
class Solution {
  public int[][] imageSmoother(int[][] M) {
    if (M.length == 0 || M[0].length == 0) return M;
    int[][] s = new int[M.length][M[0].length];
    for (int i = 0; i < M.length; i++) {
      for (int j = 0; j < M[i].length; j++) {
        s[i][j] = smooth(M, i, j);
      }
    }
    return s;
  }

  private int smooth(int[][] img, int row, int col) {
    if (row < 0 || row >= img.length || col < 0 || col >= img[row].length) return -1;
    int p = 0;
    int counter = 0;
    for (int i = -1; i <= 1; i++) {
      if (row + i >= 0 && row + i < img.length) {
        if (col - 1 >= 0) {
          p += img[row + i][col - 1];
          counter++;
        }
        p += img[row + i][col];
        counter++;
        if ( col + 1 < img[row + i].length) {
          p += img[row + i][col + 1];
          counter++;
        }                
      }
    }
    return p / counter;
  }
}
```
