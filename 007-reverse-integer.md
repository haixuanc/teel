# [7. Reverse Integer](https://leetcode.com/problems/reverse-integer/)

Reverse digits of an integer.

Example1: x = 123, return 321
Example2: x = -123, return -321

click to show spoilers.

Have you thought about this?
Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!

If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.

Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?

For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

Update (2014-11-10):
Test cases had been added to test the overflow behavior.

## Solution 1

Time: O(n)

Space: O(lgn) number of bits to store the reverse integer

```java
public class Solution {
  public int reverse(int x) {
    int y = 0;
    while (x != 0) {
      if ((x > 0 && y > Integer.MAX_VALUE / 10) || (x < 0 && y < Integer.MIN_VALUE / 10)) return 0;
      y = y * 10 + x % 10;
      x /= 10;
    }
    return y;
  }
}
```

## How to check for integer overflow

The catch is how to check for **integer overflow**.

Suppose for a valid positive integer x, y[..., n] is the reverse integer for x[n, ...], the n rightmost digits of x.  If x still has some remaining digits unexplored and y does not overflow, then y[..., n] * 10 + x[n+1] <= max. Since x[n+1] >= 0, y[.., n] * 10 <= max => y[.., n] <= max/10. Conversely, if y[.., n] > max/10, the reverse for x[n + 1, …] will overflow.
The above property can act as the loop invariant of your program.
