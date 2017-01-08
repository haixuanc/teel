# [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.



Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:
Although the above answer is in lexicographical order, your answer could be in any order you want.

## Solution 1. Recursion

Assume f(n) = all possible letter combinations for substring[0, n)

f(0) = [ "" ]

f(n) = [ for every s in f(n - 1) , for every c in char[s[n - 1]] =>  s + c ]

Time: O(3 ^ n)

Space: O(3 ^ n)

```java
public class Solution {
  private final char[][] chars = {
    { ' ' },
    {},
    { 'a', 'b', 'c' },
    { 'd', 'e', 'f' },
    { 'g', 'h', 'i' },
    { 'j', 'k', 'l' },
    { 'm', 'n', 'o' },
    { 'p', 'q', 'r', 's' },
    { 't', 'u', 'v' },
    { 'w', 'x', 'y', 'z' }
  };

  public List<String> letterCombinations(String digits) {
    if (digits.length() == 0) return new ArrayList<String>();
    return trans(digits.toCharArray(), digits.length());
  }

  private List<String> trans(char[] digits, int len) {
    if (len <= 0 || len > digits.length || digits[len - 1] < '0' || digits[len - 1] > '9') return Arrays.asList("");
    List<String> letters = new ArrayList<String>();
    for (String s : trans(digits, len - 1)) {
      for (char c : chars[digits[len - 1] - '0']) {
        letters.add(s + c);
      }
    }
    return letters;
  }
}
```

## Solution 2. Backtrace/DFS

» Backtrace = DFS + a shared variable for storing one final result, and the variable is used by all recursive function calls

Use a shared variable to store the final result through the recursive call stack. Find one final result using DFS. Since the variable is shared by the upper function call to generate another result, we have to **reset** the variable every time a function call returns.

Time: O(3^n)

Space: O(3^n)

```java
public class Solution {
  private final char[][] chars = {
    { ' ' },
    {},
    { 'a', 'b', 'c' },
    { 'd', 'e', 'f' },
    { 'g', 'h', 'i' },
    { 'j', 'k', 'l' },
    { 'm', 'n', 'o' },
    { 'p', 'q', 'r', 's' },
    { 't', 'u', 'v' },
    { 'w', 'x', 'y', 'z' }
  };

  public List<String> letterCombinations(String digits) {
    List<String> letters = new ArrayList<String>();
    if (digits.length() == 0) return letters;
    trans(letters, new StringBuilder(), digits.toCharArray(), 0);
    return letters;
  }

  private void trans(List<String> letters, StringBuilder s, char[] digits, int i) {
    if (i == digits.length) {
      letters.add(s.toString());
      return;
    }
    if (digits[i] < '0' || digits[i] > '9') return;
    for (char c : chars[digits[i] - '0']) {
      s.append(c);
      trans(letters, s, digits, i + 1);
      s.deleteCharAt(s.length() - 1);
    }
  }
}
```

## Solution 3. Iteration/BFS

```java
public class Solution {
  public List<String> letterCombinations(String digits) {
    if (digits.length() == 0) return new ArrayList<String>();
    String[] mappings = { "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };
    List<String> letters = new ArrayList<String>();
    letters.add("");
    for (char d : digits.toCharArray()) {
      if (d < '0' || d > '9') break;
      List<String> next = new ArrayList<String>();
      for (String s : letters) {
        for (char c : mappings[d - '0'].toCharArray()) {
          next.add(s + c);
        }
      }
      letters.clear();
      letters.addAll(next);
    }
    return letters;
  }
}
```

### A small technique: modify items in a list while traversing it

Use the logical loop invariant of the algorithm to update items in a list while traversing it.

```java
public class Solution {
  public List<String> letterCombinations(String digits) {
    if (digits.length() == 0) return new ArrayList<String>();
    String[] mappings = { "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };
    List<String> letters = new ArrayList<String>(); // Use list as a FIFO queue
    letters.add("");
    for (int i = 0; i < digits.length(); i++) {
      int d = digits.charAt(i) - '0';
      if (d < 0 || d > 9) break;
      while (i == letters.get(0).length()) {
        String s = letters.remove(0);
        for (char c : mappings[d].toCharArray()) {
          letters.add(s + c);
        }
      }
    }
    return letters;
  }
}
```
