# [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/?tab=Description)

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

Example 1:
Input:

"bbbab"
Output:
4
One possible longest palindromic subsequence is "bbbb".
Example 2:
Input:

"cbbd"
Output:
2
One possible longest palindromic subsequence is "bb".

## Solution 1. Bottom-up DP

- f(i, j) = 0, i > j
- f(i, j) = 1, i == j
- f(i, j) = 2 + f(i + 1, j - 1), if s[i] == s[j], a proof by contradiction can prove it
- f(i, j) = max(f(i + 1, j), f(i, j - 1)), otherwise

Time: O(n ^ 2)

Space: O(n ^ 2)

```java
public class Solution {
  public int longestPalindromeSubseq(String s) {
    if (s.length() <= 1) return s.length();
    int[][] res = new int[s.length()][s.length()];
    for (int i = 0; i < s.length(); i++) res[i][i] = 1;
    for (int i = 1; i < s.length(); i++) {
      for (int j = 0, k = i; k < s.length(); j++, k++) {
        res[j][k] = s.charAt(j) == s.charAt(k) ? 2 + res[j + 1][k - 1] : Math.max(res[j + 1][k], res[j][k - 1]);
      }
    }
    return res[0][s.length() - 1];
  }
}
```
