# [38. Count and Say](https://leetcode.com/problems/count-and-say/)

The count-and-say sequence is the sequence of integers beginning as follows:
1, 11, 21, 1211, 111221, ...

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.
Given an integer n, generate the nth sequence.

Note: The sequence of integers will be represented as a string.

## Solution 1

Time: O(n)

Space: O(n) ? unknown, needs formal mathematical proof 

```java
public class Solution {
  public String countAndSay(int n) {
    String code = "1";
    while (--n > 0) {
      code = encode(code);
    }
    return code;
  }

  private String encode(String s) {
    if (s.length() == 0) return s;
    char last = s.charAt(0);
    int count = 0;
    StringBuilder code = new StringBuilder();
    for (char c : s.toCharArray()) {
      if (c == last) {
        count++;
      } else {
        code.append(count + "" + last);
        last = c;
        count = 1;
      }
    }
    code.append(count + "" + last);
    return code.toString();
  }
}
```
