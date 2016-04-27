# [326. Power of Three](https://leetcode.com/problems/power-of-three/)

Given an integer, write a function to determine if it is a power of three.

Follow up:
Could you do it without using any loop / recursion?

## Solution 1. Iteration

Time: log(n)

```java
public class Solution {
    public boolean isPowerOfThree(int n) {
		while (n > 1) {
			if (n % 3 != 0) return false;
			n /= 3;
		}
		return n == 1;
    }
}
```

## Solution 2. Math

If n = 3 ^ k, then log(n, 3) = k where k is an integer.
Java only offers log10 and logE. log(n, 3) = log(n, 10) / log(n, 3).

```java
public class Solution {
    public boolean isPowerOfThree(int n) {
		return (Math.log10(n) / Math.log10(3)) % 1 == 0;
    }
}
```
