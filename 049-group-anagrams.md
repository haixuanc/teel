# [49. Group Anagrams](https://leetcode.com/problems/anagrams/)

Given an array of strings, group anagrams together.

For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"], 
Return:

[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]

Note: All inputs will be in lower-case.

## Solution 1. Hash table plus sort strings

Time: O(nlgn)

Space: O(n)

```java
public class Solution {
  public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> groups = new HashMap<String, List<String>>();
    for (String s : strs) {
      char[] sorted = s.toCharArray();
      Arrays.sort(sorted);
      String anagram = new String(sorted);
      if (!groups.containsKey(anagram)) groups.put(anagram, new ArrayList<String>());
      groups.get(anagram).add(s);
    }
    return new ArrayList<List<String>>(groups.values());
  }
}
```
