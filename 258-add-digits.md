# [258. Add Digit](https://leetcode.com/problems/add-digits/)

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

For example:

Given num = 38, the process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return it.

Follow up:
Could you do it without any loop/recursion in O(1) runtime?

Hint:

- A naive implementation of the above process is trivial. Could you come up with other methods?
- What are all the possible results?
- How do they occur, periodically or randomly?
- You may find this [Wikipedia article](https://www.wikiwand.com/en/Digital_root) useful.

## Solution 1. Digital root

[Wikipedia of digital root](https://www.wikiwand.com/en/Digital_root)

Proof of digital root existence:

- Assume s(num) =s(num, 0) = d0 + d1 + .. + dn, if num = d0 + 10 * d1, 10^2 * d2, .. + 10^n * dn
- s(num, j) = s(s(num, j - 1))
- s(num, j) <= s(num, j - 1)
  - s(num, j) = s(num, j - 1), if d1, d2, .., dn all equal to zero
  - s(num, j) < s(num, j - 1), otherwise
- Therefore, at each iteration num reduces by at least one. Eventually, num will become one digit.

Proof of digital root of a number is its 9 modulo:

10^k % 9 = k % 9

(d0 + d1 * 10 + .. + dn * 10^n) % 9 = (d0 + d1 + .. + dn) % 9

```java
public class Solution {
    public int addDigits(int num) {
		if (num == 0) return 0;
		num %= 9;
		return num == 0 ? 9 : num;
    }
}
```

## Solution 2

Negative modulo in Java:

-n % m = -(n % m)

```java
public class Solution {
    public int addDigits(int num) {
		return 1 + (num - 1) % 9;
    }
}
```
