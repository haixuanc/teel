# [387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

Examples:

s = "leetcode"
return 0.

s = "loveleetcode",
return 2.

Note: You may assume the string contain only lowercase letters.

## Solution 1. Frequency table

We have to determine two things for each character:

- The character is in the string
- The character appears exactly once

Time: O(n)

Space: O(k), size of the character set

```java
public class Solution {
  public int firstUniqChar(String s) {
    if (s.length() == 0) return -1;
    int[] freq = new int[26];
    for (char c : s.toCharArray()) freq[c - 'a']++;
    for (int i = 0; i < s.length(); i++) {
      if (freq[s.charAt(i) - 'a'] == 1) return i;
    }
    return -1;
  }
}
```
