# [66. Plus One](https://leetcode.com/problems/plus-one/)

Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.

## Solution 1. Naively scan all digits backward

Time: O(n)

Space: O(1) or O(n) if original number is 99...9

```java
public class Solution {
  public int[] plusOne(int[] digits) {
    if (digits.length == 0) return digits;
    int carry = 1;
    for (int i = digits.length - 1; i >= 0; i--) {
      digits[i] += carry;
      carry = digits[i] > 9 ? 1 : 0;
      digits[i] %= 10;
    }
    if (carry != 0) {
      digits = new int[digits.length + 1];
      digits[0] = 1;
    }
    return digits;
  }
}
```

## Solution 2. Stop early if no carry is produced

We don't have to scan all digits. Instead we only need to examine the next digit if a carrying occurs after adding one to the current digit. Otherwise we can safely terminate the loop.

```java
public class Solution {
  public int[] plusOne(int[] digits) {
    if (digits.length == 0) return digits;
    for (int i = digits.length - 1; i >= 0; i--) {
      if (++digits[i] <= 9) return digits;
      digits[i] = 0;
    }
    digits = new int[digits.length + 1];
    digits[0] = 1;
    return digits;
  }
}
```
