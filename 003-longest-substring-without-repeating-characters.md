# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

## Solution 1. Two pointers

We use two pointers x and y to track the start and end of the longest substring without duplicates that starts from x. If next character already exists in substring [x, y], we update the max substring length and increment x until [x, y + 1] contains no duplicates. We will repeat this process until y exceeds the length of the string.

Time: O(n^2)

Space: O(1)

Loop invariant: s[i, j) is the longest substring ending at index j - 1 that has no duplicates.

- If s[j] != any of s[i, j), increment j by one.
- Otherwise, increment i until [i, j] contains no duplicates.

```java
public class Solution {
  public int lengthOfLongestSubstring(String s) {
    int len = 0, i = 0, j = -1;
    while (++j < s.length()) {
      for (int k = i; k < j; k++) {
        if (s.charAt(k) == s.charAt(j)) {
          len = Math.max(len, j - i);
          i = k + 1;
          break;
        }
      }
    }
    return Math.max(len, j - i);
  }
}
```

## Solutino 2. Hash table

If we want to save traversal time, we can store the last position of each unique character seen so far. If the next character already exists and it is within the current substring, we will update the start of the substring to exclude the old duplicate character to preserve the loop invariant that the current substring contains only unique characters.

Time: O(n)

Space: O(n), number of unique characters in the string

```java
public class Solution {
  public int lengthOfLongestSubstring(String s) {
    int len = 0, i = 0, j = -1;
    Map<Character, Integer> lastSeen = new HashMap<Character, Integer>();
    while (++j < s.length()) {
      char c = s.charAt(j);
      if (lastSeen.containsKey(c) && lastSeen.get(c) >= i) {
        len = Math.max(len, j - i);
        i = lastSeen.get(c) + 1;
      }
      lastSeen.put(c, j);
    }
    return Math.max(len, j - i);
  }
}
```
