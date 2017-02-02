# [126. Word Ladder II](https://leetcode.com/problems/word-ladder-ii/)

Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:

Only one letter can be changed at a time
Each transformed word must exist in the word list. Note that beginWord is not a transformed word.
For example,

Given:

```
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log","cog"]
```

Return

```
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]
```

Note:

- Return an empty list if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume beginWord and endWord are non-empty and are not the same.

## Solution 1. BFS + DFS

The problem can be abstracted as to find all shortest paths connect two vertices in an un-directed graph.

**Step 1.** Use BFS to find the length of the shortest path between the two vertices.

**Step 2.** Use DFS to find all paths connecting the two vertices and only return the shortest ones.

### Optimization

The OJ is very strict about the running time, so we have to apply some optimization by storing auxiliary information to speed up the process.

- Instead of scanning the whole graph, we will only scan the graph originated at the start vertex level by level until we have visited the destination vertex. Note once we have encountered the destination vertex, we can't terminate immediately. We have to scan the rest vertices in the current level because other vertices might be adjacent to the destination vertex as well.
- We will populate the visible part of the graph as a collection of adjacency lists, and use it to construct the shortest paths instead of scanning the graph again.
- To reduce the search space when constructing the shortest paths using DFS, we will also store the distances to the start vertex for each visited vertex and construct the paths backwards.

Time: O(n + m), n = # of words, m = # of transformations

Space: O(n + m)

```java
public class Solution {
  public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
    Map<String, List<String>> adjacentLists = new HashMap<String, List<String>>();
    Map<String, Integer> distances = new HashMap<String, Integer>();
    bfs(beginWord, endWord, new HashSet<String>(wordList), adjacentLists, distances);
    if (!adjacentLists.containsKey(endWord)) return new ArrayList<List<String>>();
    List<List<String>> ladders = new ArrayList<List<String>>();
    List<String> ladder = new LinkedList<String>();
    ladder.add(endWord);
    dfs(beginWord, adjacentLists, distances, ladder, ladders);
    return ladders;
  }

  private void dfs(String begin, Map<String, List<String>> adjacentLists, Map<String, Integer> distances, List<String> ladder, List<List<String>> ladders) {
    if (ladder.get(0).equals(begin)) {
      ladders.add(new ArrayList<String>(ladder));
      return;
    }
    for (String v : adjacentLists.get(ladder.get(0))) {
      // We use distance from the start vertex to filter out previosusy visited vertices.
      if (distances.get(v) == distances.get(ladder.get(0)) - 1) {
        ladder.add(0, v);
        dfs(begin, adjacentLists, distances, ladder, ladders);
        ladder.remove(0);
      }
    }
  }

  private void bfs(String begin, String end, Set<String> dict, Map<String, List<String>> adjacentLists, Map<String, Integer> distances) {
    Queue<String> toVisit = new ArrayDeque<String>();
    toVisit.add(begin);
    // Contains words whose transformed words have all been visited.
    // It is equivalent as a vertex whose children have all been visited.
    Set<String> done = new HashSet<String>();
    adjacentLists.put(begin, new ArrayList<String>());
    distances.put(begin, 0);
    // Optimization:
    //
    // We are not going to scan the whole graph. We will only scan the
    // graph level by level until destination vertex is visited and all
    // of its adjacent vertices have been visited.
    while (!adjacentLists.containsKey(end) && !toVisit.isEmpty()) {
      for (int last = toVisit.size(); last > 0; last--) {
        String cur = toVisit.remove();
        char[] letters = cur.toCharArray();
        for (int i = 0; i < letters.length; i++) {
          char old = letters[i];
          for (char c = 'a'; c <= 'z'; c++) {
            if (c == old) continue;
            letters[i] = c;
            String next = new String(letters);
            // If the transformed word is in the dictionary, it is equivalent
            // as two nodes are connected.
            if (dict.contains(next)) {
              // If the node has not been visited yet
              if (!adjacentLists.containsKey(next)) {
                adjacentLists.put(next, new ArrayList<String>());
                distances.put(next, distances.get(cur) + 1);
                toVisit.add(next);
              }
              // We can only update the adjacency list of the next word
              // if its children has not been fully visited.
              if (!done.contains(next)) {
                adjacentLists.get(cur).add(next);
                adjacentLists.get(next).add(cur);
              }
            }
          }
          letters[i] = old;
        }
        done.add(cur);
      }
    }
  }
}
```

A more concise BFS:

```java
public class Solution {
  public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
    Map<String, List<String>> adjacentLists = new HashMap<String, List<String>>();
    Map<String, Integer> distances = new HashMap<String, Integer>();
    Set<String> dict = new HashSet<String>(wordList);
    dict.add(beginWord); // NOTE: beginWord is not in wordList
    bfs(beginWord, endWord, dict, adjacentLists, distances);
    if (!adjacentLists.containsKey(endWord)) return new ArrayList<List<String>>();
    List<List<String>> ladders = new ArrayList<List<String>>();
    List<String> ladder = new LinkedList<String>();
    ladder.add(endWord);
    dfs(beginWord, adjacentLists, distances, ladder, ladders);
    return ladders;
  }

  private void dfs(String begin, Map<String, List<String>> adjacentLists, Map<String, Integer> distances, List<String> ladder, List<List<String>> ladders) {
    if (ladder.get(0).equals(begin)) {
      ladders.add(new ArrayList<String>(ladder));
      return;
    }
    for (String v : adjacentLists.get(ladder.get(0))) {
      // We use distance from the start vertex to filter out previosusy visited vertices.
      if (distances.get(v) == distances.get(ladder.get(0)) - 1) {
        ladder.add(0, v);
        dfs(begin, adjacentLists, distances, ladder, ladders);
        ladder.remove(0);
      }
    }
  }

  private void bfs(String begin, String end, Set<String> dict, Map<String, List<String>> adjacentLists, Map<String, Integer> distances) {
    distances.put(begin, 0);
    adjacentLists.put(begin, new ArrayList<String>());
    Queue<String> toVisit = new ArrayDeque<String>();
    toVisit.add(begin);
    while (!toVisit.isEmpty()) {
      String cur = toVisit.remove();
      char[] letters = cur.toCharArray();
      for (int i = 0; i < letters.length; i++) {
        char old = letters[i];
        for (char c = 'a'; c <= 'z'; c++) {
          if (c == old) continue;
          letters[i] = c;
          String next = new String(letters);
          // If the transformed word is in the dictionary, it is equivalent
          // as two nodes are connected.
          if (dict.contains(next)) {
            adjacentLists.get(cur).add(next);
            if (!adjacentLists.containsKey(next)) {
              adjacentLists.put(next, new ArrayList<String>());
              distances.put(next, distances.get(cur) + 1);
              toVisit.add(next);
            }
          }
        }
        letters[i] = old;
      }
      if (cur.equals(end)) return;
    }
  }
}
```
