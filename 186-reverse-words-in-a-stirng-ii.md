# [186. Reverse Words in a String II](https://leetcode.com/problems/reverse-words-in-a-string-ii/)

Given an input string, reverse the string word by word. A word is defined as a sequence of non-space characters.

The input string does not contain leading or trailing spaces and the words are always separated by a single space.

For example,
Given s = "the sky is blue",
return "blue is sky the".

Could you do it in-place without allocating extra space?

## Solution 1. Two pointers

Time: O(n)

Space: O(1)

```java
public class Solution {
  public void reverseWords(char[] s) {
    for (int i = 0, j = s.length - 1; i < j; i++, j--) {
      char c = s[i];
      s[i] = s[j];
      s[j] = c;
    }
    for (int i = 0, j = 0; i < s.length; i = j + 1) {
      while (i < s.length && s[i] == ' ') i++;
      for (j = i; j < s.length && s[j] != ' '; j++) {}
      j--;
      for (int p = i, q = j; p < q; p++, q--) {
        char c = s[p];
        s[p] = s[q];
        s[q] = c;
      }
    }
  }
}
```
