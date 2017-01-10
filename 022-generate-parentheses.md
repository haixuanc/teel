# [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]

## Solution 1. Recursion

Initialization:

left = right

Loop invariant:

- if left > 0, append (
- if right > 0, append )

Termination: 

- if left > right, invalid sequence of parentheses
- if right = 0, generate one valid sequence

Time: O(2^n)

Space: O(2^n)

```java
public class Solution {
  public List<String> generateParenthesis(int n) {
    List<String> strs = new ArrayList<String>();
    if (n == 0) return strs;
    generate(strs, new StringBuilder(), n, n);
    return strs;
  }

  private void generate(List<String> strs, StringBuilder s, int left, int right) {
    if (left > right) return;
    if (right == 0) {
      strs.add(s.toString());
      return;
    }
    if (left > 0) {
      s.append('(');
      generate(strs, s, left - 1, right);
      s.deleteCharAt(s.length() - 1);
    }
    if (right > 0) {
      s.append(')');
      generate(strs, s, left, right - 1);
      s.deleteCharAt(s.length() - 1);
    }
  }
}
```

## Solution 2. DP

Suppose f(n) = all valid n pairs of parentheses

- f(0) = ""
- f(n) = (f(n - 1))f(0) + (f(n - 2))f(1) + (f(n - 3))f(2) + … + (f(0))f(n - 1)
- Or f(n) = f(n - 1)(f(0)) + f(n - 2)(f(1)) + … + f(0)(f(n - 1))

```java
public class Solution {
  public List<String> generateParenthesis(int n) {
    if (n == 0) return new ArrayList<String>();
    List<List<String>> seqs = new ArrayList<List<String>>();
    seqs.add(Arrays.asList(""));
    for (int i = 1; i <= n; i++) {
      seqs.add(new ArrayList<String>());
      for (int j = i - 1; j >= 0; j--) {
        for (String p : seqs.get(j)) {
          for (String q : seqs.get(i - 1 - j)) {
            seqs.get(i).add('(' + p + ')' + q);
          }
        }
      }
    }
    return seqs.get(n);
  }
}
```
