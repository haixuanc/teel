# [140. Word Break II](https://leetcode.com/problems/word-break-ii/)

Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. You may assume the dictionary does not contain duplicate words.

Return all such possible sentences.

For example, given
s = "catsanddog",
dict = ["cat", "cats", "and", "sand", "dog"].

A solution is ["cats and dog", "cat sand dog"].

## Solution 1. DP + backtracing

**Step 1.** DP to find word breaks

**Step 2.** Backtracing to find sentences

Time: O(nm)

Space: O(n)

```java
public class Solution {
  public List<String> wordBreak(String s, List<String> wordDict) {
    if (s.length() == 0 || wordDict.size() == 0) return new ArrayList<String>();
    boolean[] hasBreak = new boolean[s.length() + 1];
    hasBreak[0] = true;
    for (int i = 0; i < hasBreak.length; i++) {
      if (!hasBreak[i]) continue;
      for (String w : wordDict) {
        if (s.substring(i).startsWith(w)) hasBreak[i + w.length()] = true;
      }
    }
    if (!hasBreak[hasBreak.length - 1]) return new ArrayList<String>();
    List<String> sentences = new ArrayList<String>();
    dfs(sentences, new ArrayList<String>(), new HashSet<String>(wordDict), s, hasBreak, 0);
    return sentences;
  }

  private void dfs(List<String> sentences, List<String> words, Set<String> dict, String s, boolean[] hasBreak, int start) {
    if (start == s.length()) {
      sentences.add(String.join(" ", words));
      return;
    }
    for (int i = start + 1; i < hasBreak.length; i++) {
      if (!hasBreak[i] || !dict.contains(s.substring(start, i))) continue;
      words.add(s.substring(start, i));
      dfs(sentences, words, dict, s, hasBreak, i);
      words.remove(words.size() - 1);
    }
  }
}
```

Another way to do backtracing:

```java
public class Solution {
  public List<String> wordBreak(String s, List<String> wordDict) {
    if (s.length() == 0 || wordDict.size() == 0) return new ArrayList<String>();
    boolean[] hasBreak = new boolean[s.length() + 1];
    hasBreak[0] = true;
    for (int i = 0; i < hasBreak.length; i++) {
      if (!hasBreak[i]) continue;
      for (String w : wordDict) {
        if (s.substring(i).startsWith(w)) hasBreak[i + w.length()] = true;
      }
    }
    if (!hasBreak[hasBreak.length - 1]) return new ArrayList<String>();
    List<String> sentences = new ArrayList<String>();
    dfs(sentences, new ArrayList<String>(), s, wordDict, 0);
    return sentences;
  }

  private void dfs(List<String> sentences, List<String> words, String s, List<String> dict, int i) {
    if (i == s.length()) {
      sentences.add(String.join(" ", words));
      return;
    }
    for (String w : dict) {
      if (s.substring(i).startsWith(w)) {
        words.add(w);
        dfs(sentences, words, s, dict, i + w.length());
        words.remove(words.size() - 1);
      }
    }
  }
}
```

## Solution 2. DP

Recursion + memoization

```java
public class Solution {
  public List<String> wordBreak(String s, List<String> wordDict) {
    if (s.length() == 0 || wordDict.size() == 0) return new ArrayList<String>();
    boolean[] hasBreak = new boolean[s.length() + 1];
    hasBreak[0] = true;
    for (int i = 0; i < hasBreak.length; i++) {
      if (!hasBreak[i]) continue;
      for (String w : wordDict) {
        if (s.substring(i).startsWith(w)) hasBreak[i + w.length()] = true;
      }
    }
    if (!hasBreak[hasBreak.length - 1]) return new ArrayList<String>();
    return breakString(new HashMap<Integer, List<String>>(), wordDict, s, 0);
  }

  private List<String> breakString(Map<Integer, List<String>> breaks, List<String> dict, String s, int i) {
    if (breaks.containsKey(i)) return breaks.get(i);
    if (i == s.length()) {
      breaks.put(i, Arrays.asList(""));
      return breaks.get(i);
    }
    breaks.put(i, new ArrayList<String>());
    for (String w : dict) {
      if (s.substring(i).startsWith(w)) {
        for (String p : breakString(breaks, dict, s, i + w.length())) breaks.get(i).add(p.length() == 0 ? w : w + ' ' + p);
      }
    }
    return breaks.get(i);
  }
}
```
