# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

## Solution 1. Stack plus hash table

Time: O(n)

Space: O(n)

```java
public class Solution {
  public boolean isValid(String s) {
    if (s.length() % 2 == 1) return false;
    Map<Character, Character> brackets = new HashMap<Character, Character>();
    brackets.put(']', '[');
    brackets.put(')', '(');
    brackets.put('}', '{');
    Deque<Character> openings = new ArrayDeque<Character>();
    for (int i = 0; i < s.length(); ++i) {
      switch (s.charAt(i)) {
        case '[':
        case '(':
        case '{':
          openings.push(s.charAt(i));
          break;
        case ']':
        case ')':
        case '}':
          if (openings.isEmpty() || openings.removeFirst() != brackets.get(s.charAt(i))) return false;
          break;
        default:
          return false;
      }
    }
    return openings.isEmpty();
  }
}
```
