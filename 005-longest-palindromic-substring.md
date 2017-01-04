# [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example:

Input: "babad"

Output: "bab"

Note: "aba" is also a valid answer.
Example:

Input: "cbbd"

Output: "bb"

## Solution 1. Find longest palindromic substring centered at each character

We iterate through each character in the string and find the longest palindromic substring that centers at the current character.

One optimization: assume x is the current character, y is the character before x, z is the character after x, and x != y. If  z == x, they will form the center for the longest palindromic substring. Why? Because y != x == z, so x cannot be the center for a palindromic substring and only xz together can be the center.

Time: O(n^2)

Space: O(n)

```java
public class Solution {
  public String longestPalindrome(String s) {
    if (s.length() <= 1) return s;
    String max = "";
    // Loop invariant:
    //
    // s[i] != s[i - 1]
    // s[j, k] is a palindromic substring that centers at s[i]
    //
    // Since s[i] != s[i - 1], the longest possible palindromic
    // substring must center at s[i], and its length can be as long
    // as min(i * 2 + 1, (len - 1 - i) * 2 + 1)
    for (int i = 0; Math.min((i << 1) + 1, ((s.length() - 1 - i) << 1) + 1) > max.length(); ) {
      int j = i, k = i;
      while (k < s.length() && s.charAt(k) == s.charAt(i)) k++;
      i = k--;
      while (j >= 0 && k < s.length() && s.charAt(j) == s.charAt(k)) {
        j--;
        k++;
      }
      if (k - ++j > max.length()) max = s.substring(j, k);
    }
    return max;
  }
}
```

## Solution 2. Bottom-up dynamic programming

Assume p[i, j] == true if substring s[i, j] is a palindrome.

- p[i, i] = true, for i in [0, len)
- p[i, j] = s[i] == s[j], if j = i + 1
- p[i, j] = s[i] == s[j] && p[i + 1, j - 1] == true, otherwise

The base cases are substrings with length of one or two.

Time: O(n^2)

Space: O(n^2)

```java
public class Solution {
  public String longestPalindrome(String s) {
    if (s.length() <= 1) return s;
    int start = -1, end = -1;
    boolean[][] isPal = new boolean[s.length()][s.length()];
    for (int size = 1; size <= s.length(); size++) {
      for (int i = 0; i + size - 1 < s.length(); i++) {
        if ((isPal[i][i + size - 1] = s.charAt(i) == s.charAt(i + size - 1) && (size <= 2 || isPal[i + 1][i + size - 2])) && size > (end - start)) {
          start = i;
          end = i + size;
        }
      }
    }
    return s.substring(start, end);
  }
}
```
