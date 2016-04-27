# [29. Divide Two Integers](https://leetcode.com/problems/divide-two-integers/)

Divide two integers without using multiplication, division and mod operator.

If it is overflow, return MAX_INT.

## Solution. Binary search based logrithmic calculation

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

## Two's compliment respresentation in Java

[0/1][xx..x]

- Integer.MAX_VALUE = 2 ^ 31 - 1 = 011..1
- Integer.MIN_VALUE = 2 ^ 31 = 100..0 = -(flip every bit, then plus one)
