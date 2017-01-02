# [58. Length of Last Word](https://leetcode.com/problems/length-of-last-word/)

Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

For example, 
Given s = "Hello World",
return 5.

## Solution 1. Two pointers

Use two pointers to track the start and end of the last word.

Time: O(n)

Space: O(1)

```java
public class Solution {
  public int lengthOfLastWord(String s) {
    // Loop invariant:
    //
    // i points to the position after the last character of the last word
    // j is the first character of the last word
    int i = s.length();
    for (int j = s.length() - 1; j >= 0; j--) {
      if (s.charAt(j) == ' ' && j == i - 1) {
        i--;
      } else if (s.charAt(j) == ' ') {
        return i - j - 1;
      }
    }
    return i;
  }
}
```
