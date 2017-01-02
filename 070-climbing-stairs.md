# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

## Solution 1. Naive recursion

Time: O(2^n)

Space: O(2^n)

```java
public class Solution {
  public int climbStairs(int n) {
    if (n < 0) return 0;
    if (n == 0) return 1;
    return climbStairs(n - 1) + climbStairs(n - 2);
  }
}
```

## Solution 2. Bottom-up DP

Time: O(n)

Space: O(1)

```java
public class Solution {
  public int climbStairs(int n) {
    if (n < 0) return 0;
    int x = 0, y = 1;
    while (n-- > 0) {
      y += x;
      x = y - x;
    }
    return y;
  }
}
```
