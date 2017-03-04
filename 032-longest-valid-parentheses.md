# [32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/?tab=Description)

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

For "(()", the longest valid parentheses substring is "()", which has length = 2.

Another example is ")()())", where the longest valid parentheses substring is "()()", which has length = 4.

## Solution 1. DP

f(i): length of the longest valid parenthesis subsequence ending at i

Time: O(n)

Space: O(n)

```java
public class Solution {
  public int longestValidParentheses(String s) {
    if (s.length() <= 1) return 0;
    int[] longest = new int[s.length()];
    int max = 0;
    for (int i = 1; i < s.length(); i++) {
      if (s.charAt(i) == ')' && (i - 1 - longest[i - 1] >= 0 && s.charAt(i - 1 - longest[i - 1]) == '(')) {
        longest[i] = longest[i - 1] + 2 + (i - 2 - longest[i - 1] >= 0 ? longest[i - 2 - longest[i - 1]] : 0);
      }
      max = Math.max(max, longest[i]);
    }
    return max;
  }
}
```
