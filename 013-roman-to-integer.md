# [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

## Solution 1. Hashmap

Roman numeral rules:

Symbol | Value
-------|------
I | 1
V | 5
X | 10
L | 50
C | 100
D | 500
M | 1000

Special cases:

Symbol | Value
-------|------
IV | 4
IX | 9
XL | 40
XC | 90
CD | 400
CM | 900

Time: O(n)

Space: O(1)

```java
public class Solution {
  public int romanToInt(String s) {
    if (s.length() == 0) return 0;
    Map<Character, Integer> roman = new HashMap<Character, Integer>();
    roman.put('I', 1);
    roman.put('V', 5);
    roman.put('X', 10);
    roman.put('L', 50);
    roman.put('C', 100);
    roman.put('D', 500);
    roman.put('M', 1000);
    int val = 0;
    for (int i = 0; i < s.length() - 1; i++) {
      switch (s.charAt(i)) {
        case 'I':
        case 'X':
        case 'C':
          if (roman.get(s.charAt(i)) < roman.get(s.charAt(i + 1))) {
            val -= roman.get(s.charAt(i));
            break;
          }
        default:
          val += roman.get(s.charAt(i));
          break;
      }
    }
    return val += roman.get(s.charAt(s.length() - 1));
  }
}
```
