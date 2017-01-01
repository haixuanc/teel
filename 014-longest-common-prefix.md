# [14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

Write a function to find the longest common prefix string amongst an array of strings.

## Solution 1. Iterate over all strings

Time: O(nm)

Space: O(m)

```java
public class Solution {
  public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    StringBuilder lcp = new StringBuilder();
    for (int i = 0; i < strs[0].length(); ++i) {
      for (int j = 1; j < strs.length; ++j) {
        if (i >= strs[j].length() || strs[j].charAt(i) != strs[0].charAt(i)) return lcp.toString();
      }
      lcp.append(strs[0].charAt(i));
    }
    return strs[0];
  }
}
```

## Solution 2. Sort strings first

If strings are sorted, and string x <= string y, then all of x prefices are also prefix for y. It implies that all of x prefices are also prefix for all other strings. So the longest common prefix between the smallest and greatest strings is the longest common prefix for all strings.

Time: O(nlgn) assuming comparing two strings costs O(1) time

Space: O(m)

```java
public class Solution {
  public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    if (strs.length == 1) return strs[0];
    Arrays.sort(strs);
    StringBuilder lcp = new StringBuilder();
    for (int i = 0; i < strs[0].length() && i < strs[strs.length - 1].length(); ++i) {
      if (strs[0].charAt(i) == strs[strs.length - 1].charAt(i)) lcp.append(strs[0].charAt(i));
      else return lcp.toString();
    }
    // or return lcp.toString();
    return strs[0].length() > strs[strs.length - 1].length() ? strs[strs.length - 1] : strs[0];
  }
}
```
