# [6. ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/)

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this conversion given a number of rows:

string convert(string text, int nRows);
convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

## Solution 1. One buffer for each row

Time: O(n)

Space: O(n)

Loop: for every character in the input string put it into the right row.
Output: concatenate all rows

```java
public class Solution {
  public String convert(String s, int numRows) {
    if (s.length() == 0 || numRows <= 1) return s;
    StringBuilder[] rows = new StringBuilder[numRows];
    for (int i = 0; i < numRows; i++) rows[i] = new StringBuilder();
    for (int i = 0, j = 0, inc = -1; i < s.length(); i++, j += inc) {
      rows[j].append(s.charAt(i));
      if (j % (numRows - 1) == 0) inc *= -1;
    }
    for (int i = 1; i < numRows; i++) rows[0].append(rows[i]);
    return rows[0].toString();
  }
}
```
