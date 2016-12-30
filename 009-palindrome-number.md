# [9. Palindrome Number](https://leetcode.com/problems/palindrome-number/)

Determine whether an integer is a palindrome. Do this without extra space.

click to show spoilers.

Some hints:
Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

## Solution 1. Reverse the entire number

A straight-forward solution is to reverse the entire number and compare if the reverse equals the original. But we need to be careful that the reverse of an integer could overflow.

## Solution 2. Reverse only half of the number

Another definition of a palindrome number is its first half is the reverse of its second half. I.e. a palindrome can be represented as:

- y
- xyx'
- xx'

where x' is the reverse of x.

How do we know that we have traversed half of a number?

If we use x to represent the left half of a number and y to represent the reverse of the right half. Once x == y or x < y, we know we have passed the middle digit of the original number.

Since we will only reverse half of an integer, we are exempt of the concern of integer overflow.

Time: O(n)

Space: O(lgn)

```java
public class Solution {
  public boolean isPalindrome(int x) {
    if (x < 0 || (x > 0 && x % 10 == 0)) return false;
    int y = 0;
    while (x > y) {
      y = y * 10 + x % 10;
      x /= 10;
    }
    return x == y || x == y / 10;
  }
}
```
