# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

Given a string, sort it in decreasing order based on the frequency of characters.

Example 1:

Input:
"tree"

Output:
"eert"

Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.

Example 2:

Input:
"cccaaa"

Output:
"cccaaa"

Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.

Example 3:

Input:
"Aabb"

Output:
"bbAa"

Explanation:
"bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.

## Solution 1. Frequency table plus sorted key list

Time: O(n^2)

Space: O(n)

```java
public class Solution {
  public String frequencySort(String s) {
    if (s.length() <= 2) return s;
    Map<Character, Integer> freq = new HashMap<Character, Integer>();
    for (char c : s.toCharArray()) freq.put(c, freq.containsKey(c) ? freq.get(c) + 1 : 1);
    List<Character> order = new LinkedList<Character>();
    for (char c : freq.keySet()) {
      int i = 0;
      while (i < order.size() && freq.get(c) < freq.get(order.get(i))) i++;
      order.add(i, c);
    }
    StringBuilder sorted = new StringBuilder();
    for (char c : order) {
      while (freq.get(c) > 0) {
        sorted.append(c);
        freq.put(c, freq.get(c) - 1);
      }
    }
    return sorted.toString();
  }
}
```

## Solution 2. Char-to-freq table plus freq-to-char-set table

Time: O(n)

Space: O(n)

```java
public class Solution {
  public String frequencySort(String s) {
    if (s.length() <= 2) return s;
    Map<Character, Integer> freq = new HashMap<Character, Integer>();
    for (char c : s.toCharArray()) freq.put(c, freq.containsKey(c) ? freq.get(c) + 1 : 1);
    Set<Character>[] letters = new Set[s.length() + 1];
    for (char c : freq.keySet()) {
      if (letters[freq.get(c)] == null) letters[freq.get(c)] = new HashSet<Character>();
      letters[freq.get(c)].add(c);
    }
    StringBuilder sorted = new StringBuilder();
    for (int i = letters.length - 1; i >= 0; i--) {
      if (letters[i] != null) {
        for (char c : letters[i]) {
          for (int j = i; j > 0; j--) sorted.append(c);
        }
      }
    }
    return sorted.toString();
  }
}
```

## Solution 3. Heap/PriorityQueue

Order tuples of (char, count) by count using a priority queue.

Time: O(nlgn)

Space: O(n)

```java
public class Solution {
  public String frequencySort(String s) {
    if (s.length() <= 2) return s;
    Map<Character, Integer> freq = new HashMap<Character, Integer>();
    for (char c : s.toCharArray()) freq.put(c, freq.containsKey(c) ? freq.get(c) + 1 : 1);
    // A PriorityQueue orders its elements by their natual ordering
    PriorityQueue<Map.Entry<Character, Integer>> letters = new PriorityQueue<>(new Comparator<Map.Entry<Character, Integer>>() {
        @Override
        public int compare(Map.Entry<Character, Integer> a, Map.Entry<Character, Integer> b) {
        return b.getValue() - a.getValue();
        }
        });
    letters.addAll(freq.entrySet());
    StringBuilder sorted = new StringBuilder();
    while (!letters.isEmpty()) {
      Map.Entry<Character, Integer> l = letters.remove();
      for (int i = l.getValue(); i > 0; i--) sorted.append(l.getKey());
    }
    return sorted.toString();
  }
}
```
