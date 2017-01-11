# [396. Rotate Function](https://leetcode.com/problems/rotate-function/)

Given an array of integers A and let n to be its length.

Assume Bk to be an array obtained by rotating the array A k positions clock-wise, we define a "rotation function" F on A as follow:

F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1].

Calculate the maximum value of F(0), F(1), ..., F(n-1).

Note:
n is guaranteed to be less than 105.

Example:

A = [4, 3, 2, 6]

F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26

So the maximum value of F(0), F(1), F(2), F(3) is F(3) = 26.

## Solution 1. Naive O(n^2) calculations

Time: O(n^2)

Space: O(1)

```java
public class Solution {
  public int maxRotateFunction(int[] A) {
    if (A.length == 0) return 0;
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < A.length; i++) {
      int sum = 0;
      for (int j = 0; j < A.length; j++) {
        sum += A[(i + j) % A.length] * j;
      }
      max = Math.max(max, sum);
    }
    return max;
  }
}
```

## Solution 2. Mathematical recursion

F(k) = 0 * Bk[0] + 1 * Bk[1] + 2 * Bk[2] + … + (n - 1) * Bk[n - 1]

F(k + 1) = 0 * Bk[n - 1] + 1 * Bk[0] + 2 * Bk[1] + … + (n - 1) * Bk[n - 2]

F(k + 1) = F(k) + sum(Bk) - n * Bk[n - 1] = F(k) + sum(A) - n * A[n - 1 - k]

F(0) = sum(i * A[i])

Time: O(n)

Space: O(1)

```java
public class Solution {
  public int maxRotateFunction(int[] A) {
    int total = 0;
    for (int a : A) total += a;
    int sum = 0;
    for (int i = 0; i < A.length; i++) sum += i * A[i];
    int max = sum;
    for (int i = A.length - 1; i > 0; i--) {
      sum += total - A.length * A[i];
      max = Math.max(max, sum);
    }
    return max;
  }
}
```
