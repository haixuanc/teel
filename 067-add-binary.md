# [67. Add Binary](https://leetcode.com/problems/add-binary/)

Given two binary strings, return their sum (also a binary string).

For example,
a = "11"
b = "1"
Return "100".

## Solution 1. Two pointers

Time: O(n)

Space: O(n)

```java
public class Solution {
  public String addBinary(String a, String b) {
    if (a == null || a.length() == 0) return b;
    if (b == null || b.length() == 0) return a;
    StringBuilder sum = new StringBuilder();
    int i = a.length() - 1;
    int j = b.length() - 1;
    int carrying = 0;
    while (i >= 0 || j >= 0 || carrying != 0) {
      if (i >= 0) carrying += a.charAt(i--) - '0';
      if (j >= 0) carrying += b.charAt(j--) - '0';
      sum.insert(0, carrying % 2);
      carrying >>= 1;
    }
    return sum.toString();
  }
}
```
