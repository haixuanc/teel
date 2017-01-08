# [12. Integer to Roman](https://leetcode.com/problems/integer-to-roman/)

Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

## Solution 1

Since the integer is within the small range of [1, 3999], we can use a set of static mappings to represent the rules of integers to roman numerals.

Time: O(1)

Space: O(1)

```java
public class Solution {
  public String intToRoman(int num) {
    String[] M = { "", "M", "MM", "MMM" };
    String[] C = { "", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM" };
    String [] X = { "", "X" , "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC" };
    String [] I = { "", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX" };
    return M[num / 1000] + C[(num % 1000) / 100] + X[(num % 100) / 10] + I[num % 10];
  }
}
```
