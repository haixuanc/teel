# [8. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

Implement atoi to convert a string to an integer.

Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

Update (2015-02-10):
The signature of the C++ function had been updated. If you still see your function signature accepts a const char * argument, please click the reload button  to reset your code definition.

spoilers alert... click to show requirements for atoi.

Requirements for atoi:
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.

## Solution

1. Skip any leading whitespaces.
2. One optional `+` or `-` sign.
3. Sequence of numbers.
4. Terminate and return `MAX` or `MIN` if integer overflows.
5. Terminate and return current value if a non-numerical character occurs.

Time: O(n)

Space: O(lgn)

```java
public class Solution {
  public int myAtoi(String str) {
    int i = 0;
    while (i < str.length() && str.charAt(i) == ' ') ++i;
    int sign = 1;
    if (i < str.length() && (str.charAt(i) == '+' || str.charAt(i) == '-')) {
      sign = str.charAt(i++) == '+' ? 1 : -1;
    }
    int val = 0;
    while (i < str.length() && str.charAt(i) >= '0' && str.charAt(i) <= '9') {
      int d = str.charAt(i++) - '0';
      if (val > Integer.MAX_VALUE / 10 || (val == Integer.MAX_VALUE / 10 && d > Integer.MAX_VALUE % 10)) return sign > 0 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
      val = val * 10 + d;
    }
    return val * sign;
  }
}
```

## How to check for integer overflow

Assume i is an integer, x = [..., n] the first n digits of i, y = [n + 1] the nth digit of i.

- If i is a positive integer, xy will overflow if x > MAX / 10 or x == MAX / 10 && y > MAX % 10.
- If i is a negative integer, xy will overflow if x < MIN / 10 or x == MIN / 10 && y < MIN % 10.
