# [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

The order of output does not matter.

Example 1:

Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".

Example 2:

Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".

## Solution 1. Sliding window with two frequency tables

Time: O(n)

Space: O(n)

```java
public class Solution {
  public List<Integer> findAnagrams(String s, String p) {
    if (p.length() > s.length()) return new ArrayList<Integer>();
    int[] pFreq = new int[26];
    for (char c : p.toCharArray()) pFreq[c - 'a']++;
    int[] sFreq = new int[26];
    for (char c : s.substring(0, p.length()).toCharArray()) sFreq[c - 'a']++;
    List<Integer> indices = new ArrayList<Integer>();
    if (isSame(pFreq, sFreq)) indices.add(0);
    for (int i = 1; i + p.length() <= s.length(); i++) {
      sFreq[s.charAt(i - 1) - 'a']--;
      sFreq[s.charAt(i + p.length() - 1) - 'a']++;
      if (pFreq[s.charAt(i) - 'a'] > 0 && isSame(pFreq, sFreq)) indices.add(i);
    }
    return indices;
  }

  private boolean isSame(int[] a, int[] b) {
    if (a.length != b.length) return false;
    for (int i = 0; i < a.length; i++) {
      if (a[i] != b[i]) return false;
    }
    return true;
  }
}
```

## Solution 1. Sliding window with one diff variable

### A generic thhought on two string problems

#### The problem

The problem is usually about two string s and p, where s is longer than p. We are asked to find a fixed-width substring q is s such that q matches some criteria for p.

#### A generic solution: fixed size sliding window with one state variable

We use two pointers `left` and `right` to maintain a fixed-size window, e.g. `right` - `left` = `len(p)`, and a variable `v` to describe the current state of the substring `[left, right)` in terms of the matching criteria for p. Whenever we move the pointer `left` or `right`, we will also update the state variable `v`.

Time: O(n)

Space: O(n)

```java
public class Solution {
  // `unmatched`: 
  // number of unmatched characters in p, i.e. number of elements
  // in array `freq` which are greater than zero
  // If `unmatched` = 0, current substring [i, j) is an anagram for p
  // because the size of the substring which is j - i is always equal
  // to the size of p.
  //
  // `freq`:
  // For every index (character) in `freq`:
  // - freq[c] > 0 means c is in p. We either decrement `unmatched` when
  // moving the right pointer or increment `unmatched` when moving the
  // left pointer to put the character back.
  // - freq[c] < 0 means c is not in p. The maximum value for freq[c] is
  // zero when moving the left pointer.
  //
  // `freq` is being used like a **diff** between s and p. If the diff is zero,
  // s matches p.
  public List<Integer> findAnagrams(String s, String p) {
    if (s == null || p == null || p.length() > s.length()) return new ArrayList<Integer>();
    List<Integer> indices = new ArrayList<Integer>();
    int[] freq = new int[26];
    for (char c : p.toCharArray()) freq[c - 'a']++;
    int unmatched = p.length();
    for (int i = 0, j = 0; j < s.length(); j++) {
      if (j - i == p.length() && ++freq[s.charAt(i++) - 'a'] > 0) unmatched++;
      if (freq[s.charAt(j) - 'a']-- > 0) unmatched--;
      if (unmatched == 0) indices.add(i);
    }
    return indices;
  }
}
```
