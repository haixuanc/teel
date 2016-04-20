# [290. Word Pattern](https://leetcode.com/problems/word-pattern/)

Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.

Examples:
pattern = "abba", str = "dog cat cat dog" should return true.
pattern = "abba", str = "dog cat cat fish" should return false.
pattern = "aaaa", str = "dog cat cat dog" should return false.
pattern = "abba", str = "dog dog dog dog" should return false.

Notes:
You may assume pattern contains only lowercase letters, and str contains lowercase letters separated by a single space.

## Solution. Two-way mapping

We need to maintain bi-directional mappings between letters and words:
letter <-> word

A dismatch occurs:
- if m[c] exists and m[c] != w
- or if m'[w] exists and m'[w] != c

```java
public class Solution {
    public boolean wordPattern(String pattern, String str) {
		Map<Character, String> words = new HashMap<Character, String>();
		Map<String, Character> letters = new HashMap<String, Character>();
		String[] tokens = str.split(" ");
		if (pattern.length() != tokens.length) return false;
		for (int i = 0; i < tokens.length; i++) {
			char c = pattern.charAt(i);
			String w = tokens[i];
			if ((words.containsKey(c) && !words.get(c).equals(w)) ||
					(letters.containsKey(w) && letters.get(w) != c)) return false;
			if (!words.containsKey(c)) {
				words.put(c, w);
				letters.put(w, c);
			}
		}
		return true;
    }
}

```
