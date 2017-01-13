# [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)

Given two strings s and t, write a function to determine if t is an anagram of s.

For example,
s = "anagram", t = "nagaram", return true.
s = "rat", t = "car", return false.

Note:
You may assume the string contains only lowercase alphabets.

Follow up:
What if the inputs contain unicode characters? How would you adapt your solution to such case?

## Solution 1. Sort two arrays

Time: O(nlgn), assuming we use quicksort or mergesort

Space: O(n)

```java
public class Solution {
  public boolean isAnagram(String s, String t) {
    if (s == null || t == null || s.length() != t.length()) return false;
    char[] a = s.toCharArray();
    char[] b = t.toCharArray();
    Arrays.sort(a);
    Arrays.sort(b);
    for (int i = 0; i < a.length; i++) {
      if (a[i] != b[i]) return false;
    }
    return true;
  }
}
```

## Solution 2. Compare two frequency tables

Time: O(n)

Space: O(n)

```java
public class Solution {
  public boolean isAnagram(String s, String t) {
    if (s == null || t == null || s.length() != t.length()) return false;
    int[] freq1 = new int[26];
    int[] freq2 = new int[26];
    for (int i = 0; i < s.length(); i++) {
      freq1[s.charAt(i) - 'a']++;
      freq2[t.charAt(i) - 'a']++;
    }
    for (int i = 0; i < freq1.length; i++) {
      if (freq1[i] != freq2[i]) return false;
    }
    return true;
  }
}
```

## Solution 3. Use only one frequency table

This problem is actually very similar to the **substring pattern matching problems**. S is one string, t is the *substring*. We can use only one frequency table to track the different letter between s and t. Eventually after comparing s and t. freq looks like:

freq[i] > 0 means letter i is in s but not in t

freq[i] = 0 means letter i is same for both s and t. i is in s and t or i is not in s or t.

freq[i] < 0 means letter i is in t but not in s

Time: O(n)

Space: O(n)

```java
public class Solution {
  public boolean isAnagram(String s, String t) {
    if (s == null || t == null || s.length() != t.length()) return false;
    int[] freq = new int[26];
    for (char c : s.toCharArray()) freq[c - 'a']++;
    for (char c : t.toCharArray()) {
      // If s is anagram for t, freq will eventually be all zeros.
      // Since s.length() == t.length(), it implies all letter in
      // s have been **cancelled** by a same letter in t.
      // Vice versa, if there is any different letter c in s and t,
      // freq[c] will become negative.
      if (--freq[c - 'a'] < 0) return false;
    }
    return true;
  }
}
```
