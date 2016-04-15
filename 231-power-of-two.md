# [231. Power of Two](https://leetcode.com/problems/power-of-two/)

Given an integer, write a function to determine if it is a power of two.

## Solution.

If n = ..10..0, n & ( n -1) clears the last 1 bit.

```java
public class Solution {
    public boolean isPowerOfTwo(int n) {
		return n > 0 && (n & (n - 1)) == 0;
    }
}
```
