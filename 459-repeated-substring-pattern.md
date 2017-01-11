# [459. Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)

Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

Example 1:
Input: "abab"

Output: True

Explanation: It's the substring "ab" twice.
Example 2:
Input: "aba"

Output: False
Example 3:
Input: "abcabcabcabc"

Output: True

Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)

## Solution 1

We don't have to try every substring, because the only possible candidate substrings must satisfy two requirements:

- The substring must starts at index 0, i.g. s[0, i) for i <= length / 2
- The length of the substring must be a factor of the length of the entire string

Time: O(n^2)

Space: O(1)

```java
public class Solution {
  public boolean repeatedSubstringPattern(String str) {
    for (int i = 1; i <= str.length() / 2; i++) {
      if (str.length() % i == 0) {
        int j = 0;
        for ( ; j < str.length() / i; j++) {
          if (!str.substring(0, i).equals(str.substring(i * j, i * (j + 1)))) break;
        }
        if (j == str.length() / i) return true;
      }
    }
    return false;
  }
}
```

## Solution 2. A mathematical proof

We can reduce the time of determining whether a string is a multiple of a substring from O(n) to O(1).

If a string s is a multiple of substring s[0, i), then s[0, i) = s[n - i, n) and s[0, n - i) = s[i, n). It could also be illustrated as the graph below:

string: a---...-a

s[0, n - i): a---...
s[i, n):      ---...a

substring: a

By induction:

1. a---...- = ---...-a
2. a---...-a = a---...-a
3. aa---...-aa = aa---...-aa
4. ……
5. Eventually, aa...a = aa...a

Time: O(n)

Space: O(1)

```java
public class Solution {
  public boolean repeatedSubstringPattern(String str) {
    for (int i = 1; i <= str.length() / 2; i++) {
      if (str.length() % i == 0) {
        if (str.substring(0, i).equals(str.substring(str.length() - i)) && str.substring(0, str.length() - i).equals(str.substring(i))) return true;
      }
    }
    return false;
  }
}
```
