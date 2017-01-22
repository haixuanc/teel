# [139. Word Break](https://leetcode.com/problems/word-break/)

Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words. You may assume the dictionary does not contain duplicate words.

For example, given
s = "leetcode",
dict = ["leet", "code"].

Return true because "leetcode" can be segmented as "leet code".

## Solution 1. DP

A naive top-down recursive algorithm takes O(m^n) time because the sub-problems overlap each other.

Sol 1. Top-down recursion plus O(n) memoization

Sol2. Bottom-up recursion plus O(1) memoization

### End-with word recursion

f(i) = substring s[0, i) can be segmented by words in dictionary

f(0) = true

f(i) = if substring s[0, i) ends with w and f(i - |w|) == true for w in the dictionary

Time: O(nm)

Space: O(n)

```java
public class Solution {
  public boolean wordBreak(String s, List<String> wordDict) {
    if (s.length() == 0) return false;
    boolean[] res = new boolean[s.length() + 1];
    res[0] = true;
    for (int i = 1; i < res.length; i++) {
      for (String w : wordDict) {
        if (s.substring(0, i).endsWith(w) && res[i - w.length()]) {
          res[i] = true;
          break;
        }
      }
    }
    return res[res.length - 1];
  }
}
```

### Preprocessing for optimization by skipping invalid sub-sequences/BFS

Time: O(n + km)

Space: O(n)

```java
public class Solution {
  public boolean wordBreak(String s, List<String> wordDict) {
    if (s.length() == 0) return false;
    boolean[] res = new boolean[s.length() + 1];
    res[0] = true;
    for (int i = 0; i < res.length; i++) {
      if (!res[i]) continue;
      for (String w : wordDict) {
        if (s.substring(i).startsWith(w)) res[i + w.length()] = true;
      }
    }
    return res[res.length - 1];
  }
}
```
