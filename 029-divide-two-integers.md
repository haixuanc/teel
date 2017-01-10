# [29. Divide Two Integers](https://leetcode.com/problems/divide-two-integers/)

Divide two integers without using multiplication, division and mod operator.

If it is overflow, return MAX_INT.

## Solution 1. Binary search based logrithmic calculation

If a = b * k + mod(a, b), k = 2^i + 2^j + .. + 2^q.

The original problem is to calculate k. K is an integer. Since any integer can be represented as a sum of 2's powers, we can approach k in logrithmic time.

Time: O(log(a / b))

Overflow happens when two operands are of the same sign but the result is of a different sign. When integer overflow happens, the value wraps around, e.g. Integer.MAX_VALUE + k = Integer.MIN_VALUE + (k - 1).

```java
public class Solution {
    public int divide(int dividend, int divisor) {
		if (divisor == 0) return Integer.MAX_VALUE;
		long a = Math.abs((long) dividend);
		long b = Math.abs((long) divisor);
		// NOTE: result overflows for Integer.MIN_VALUE / - 1
		long counter = 0;
		while (b <= a) {
			int i = 1;
			while ((b << i) <= a) {
				i++;
			}
			// NOTE: 1 is an integer and 1 << i may overflow
			// Use `long` 1L
			counter += 1L << --i; 
			a -= b << i;
		}
		if ((dividend & (1 << 31)) != (divisor & (1 << 31))) {
			counter *= -1;
		}
		return (int) Math.min(counter, Integer.MAX_VALUE);
    }
}
```

A straightforward idea is to approach the dividend linearly, which takes O(dividend / divisor) time. A faster way is to approach exponentially, which takes O(log(dividend / divisor)) time. We repeat this process as long as dividend is greater than or equal to divisor.

```java
public class Solution {
  public int divide(int dividend, int divisor) {
    if (divisor == 0 || (dividend == Integer.MIN_VALUE && divisor == -1)) return Integer.MAX_VALUE;
    int sign = dividend >= 0 && divisor > 0 || dividend <= 0 && divisor < 0 ? 1 : -1;
    long upper = Math.abs((long) dividend);
    long lower = Math.abs((long) divisor);
    int quotient = 0;
    while (lower <= upper) {
      long val = lower;
      long exp = 1;
      while (val <= upper) {
        val <<= 1;
        exp <<= 1;
      }
      quotient += exp >> 1;
      upper -= val >> 1;
    }
    return quotient * sign;
  }
}
```

## Solution 2. Recursive binary search

```
f(dividend, divisor) = {
  quotient: f(dividend / 2, divisor).quotient + f(dividend - dividend / 2, divisor).quotient + (f(dividend / 2, divisor).remainder + f(dividend - dividend / 2, divisor).remainder) / divisor,
  remainder: (f(...).remainder + f(...).remainder) % divisor
}
```

## Two's compliment representation in Java

[0/1][xx..x]

- sign bit = 0 : positive integer
- sign bit = 1 : negative integer

- Integer.MAX_VALUE = 2 ^ 31 - 1 = 011..1
- Integer.MIN_VALUE = 2 ^ 31 = 100..0 = -(flip every bit, then plus one)
