# [342. Power of Four](https://leetcode.com/submissions/detail/60172942/)

Given an integer (signed 32 bits), write a function to check whether it is a power of 4.

Example:
Given num = 16, return true. Given num = 5, return false.

Follow up: Could you solve it without loops/recursion?

## Solution 1. Iteration

4^k = 2^2k = 100..00, even number of trailing zeros.

```java
public class Solution {
    public boolean isPowerOfFour(int num) {
		while (num > 1) {
			if ((num & 3) != 0) return false; // Check last two digits are 00
			num >>= 2;
		}
		return num == 1;
    }
}
```

## Solution 2. No recursion

If n = 4^k = 100..00, n - 1 = 4^k - 1 = 011..11

- n ^ (n - 1) = 0
- n - 1 = 11..11, even number of ones. n - 1 = 3 * (1 + 4 + 4^2 + .. + 4^k)

```java
public class Solution {
    public boolean isPowerOfFour(int num) {
		return (num & (num -1)) == 0 && (num - 1) % 3 == 0;
    }
}
```
