# [202. Happy Number](https://leetcode.com/problems/happy-number/)

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example: 19 is a happy number

12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

[Wikipedia happy number definition](https://www.wikiwand.com/en/Happy_number)

## Solution 1

```java
public class Solution {
    public boolean isHappy(int n) {
		Set<Integer> seen = new HashSet<Integer>();
		while (n != 1 && !seen.contains(n)) {
			seen.add(n);
			n = happy(n);
		}
		return n == 1;
    }

	private int happy(int n) {
		int sum = 0;
		while (n > 0) {
			sum += (n % 10) * (n % 10);
			n /= 10;
		}
		return sum;
	}
}
```

## Solution 2. Pattern based

The sequence of any unhappy number always contains 4.

```java
public class Solution {
    public boolean isHappy(int n) {
		if (n == 1) return true;
		if (n == 4) return false;
		return isHappy(happy(n));
    }

	private int happy(int n) {
		int sum = 0;
		while (n > 0) {
			sum += (n % 10) * (n % 10);
			n /= 10;
		}
		return sum;
	}
}
```
