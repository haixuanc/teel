# [28. Implement strStr()](https://leetcode.com/problems/implement-strstr/)

Implement strStr().

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

## Solution 1. Two pointers

Time: O(nm), assuming `substring()` takes O(m)

Space: (1)

```java
public class Solution {
  public int strStr(String haystack, String needle) {
    for (int i = 0, s = needle.length(); i + s <= haystack.length(); ++i) {
      if (haystack.substring(i, i + s).equals(needle)) return i;
    }
    return -1;
  }
}
```

## Solution 2. KMP algorithm

[KMP algorithm, Wikipedia](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm)
