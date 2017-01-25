# [127. Word Ladder](https://leetcode.com/problems/word-ladder/)

Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

Only one letter can be changed at a time.
Each transformed word must exist in the word list. Note that beginWord is not a transformed word.
For example,

Given:

beginWord = "hit"

endWord = "cog"

wordList = ["hot","dot","dog","lot","log","cog"]

As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.

Note:

- Return 0 if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume beginWord and endWord are non-empty and are not the the same.

## Solution 1. BFS

The problem can be abstracted as finding the minimum distance between two nodes in a graph. Each pair of words is connected if they only differ by one letter.

Time: O(n*m^2), m is the total size of strings

Space: O(n)

```java
public class Solution {
  public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Queue<String> cur = new LinkedList<String>();
    cur.add(beginWord);
    Set<String> dict = new HashSet<String>(wordList);
    dict.remove(beginWord);
    int depth = 1;
    while (!cur.isEmpty()) {
      for (int last = cur.size(); last-- > 0; ) {
        String s = cur.remove();
        if (s.equals(endWord)) return depth;
        for (Iterator<String> i = dict.iterator(); i.hasNext(); ) {
          String t = i.next();
          if (isNext(s, t)) {
            cur.add(t);
            i.remove();
          }
        }
      }
      depth++;
    }
    return 0;
  }

  private boolean isNext(String s, String t) {
    if (s.length() != t.length()) return false;
    int diff = 0;
    for (int i = 0; i < s.length(); i++) {
      if (s.charAt(i) != t.charAt(i)) diff++;
    }
    return diff == 1;
  }
}
```

A little optimization: mutate each character of current string to generate the next possible word instead of comparing with all remaining strings.

Time: O(n*m)

Space: O(n)

```java
public class Solution {
  public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Queue<String> cur = new LinkedList<String>();
    cur.add(beginWord);
    Set<String> dict = new HashSet<String>(wordList);
    dict.remove(beginWord);
    int depth = 1;
    while (!cur.isEmpty()) {
      for (int last = cur.size(); last-- > 0; ) {
        String s = cur.remove();
        if (s.equals(endWord)) return depth;
        char[] a = s.toCharArray();
        for (int i = 0; i < a.length; i++) {
          char l = a[i];
          for (char c = 'a'; c <= 'z'; c++) {
            a[i] = c;
            String t = new String(a);
            if (dict.contains(t)) {
              cur.add(t);
              dict.remove(t);
            }
          }
          a[i] = l;
        }
      }
      depth++;
    }
    return 0;
  }
}
```

## Solution 2. Two-end BFS

To speed up BFS, we can maintain two sets of vertices which are reachable from each end. Every time, we will only exam the smaller set. After each iteration, the size of the set will double if each vertex has at most two children. The size of the bigger set will be the twice of the smaller set. Since we will always choose the smaller set, so the size of the current set will be the half of that in a normal BFS. Therefore the time complexity will be O(n/2)

Time: O(n/2)

Space: O(n/2)

```java
public class Solution {
  public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Set<String> dict = new HashSet<String>(wordList);
    Set<String> s = new HashSet<String>();
    s.add(beginWord);
    dict.remove(beginWord);
    Set<String> t = new HashSet<String>();
    t.add(endWord);
    dict.remove(endWord);
    int depth = 1;
    while (!s.isEmpty() && !t.isEmpty()) {
      if (s.size() > t.size()) {
        Set<String> u = s;
        s = t;
        t = u;
      }
      Set<String> cur = new HashSet<String>(s);
      for (String w : cur) {
        s.remove(w);
        char[] a = w.toCharArray();
        for (int i = 0; i < a.length; i++) {
          char l = a[i];
          for (char c = 'a'; c <= 'z'; c++) {
            a[i] = c;
            String next = new String(a);
            if (t.contains(next)) return depth + 1;
            if (dict.contains(next)) {
              s.add(next);
              dict.remove(next);
            }
          }
          a[i] = l;
        }
      }
      depth++;
    }
    return 0;
  }
}
```
