# [288. Unique Word Abbreviation](https://leetcode.com/problems/unique-word-abbreviation/)

An abbreviation of a word follows the form <first letter><number><last letter>. Below are some examples of word abbreviations:

a) it                      --> it    (no abbreviation)

     1
b) d|o|g                   --> d1g

              1    1  1
     1---5----0----5--8
c) i|nternationalizatio|n  --> i18n

              1
     1---5----0
d) l|ocalizatio|n          --> l10n

Assume you have a dictionary and given a word, find whether its abbreviation is unique in the dictionary. A word's abbreviation is unique if no other word from the dictionary has the same abbreviation.

Example: 
Given dictionary = [ "deer", "door", "cake", "card" ]

isUnique("dear") -> false
isUnique("cart") -> true
isUnique("cane") -> false
isUnique("make") -> true

## Solution

A word's abbreviation is unqiue:
- if no word in the dictionary has the same abbreviation;
- or the word exists in the dictionary and no other word in the dictionary has the same abbreviation.

```java
public class ValidWordAbbr {
	private Map<String, Set<String>> abbrs = new HashMap<String, Set<String>>();

    public ValidWordAbbr(String[] dictionary) {
		for (String s : dictionary) {
			String a = abbr(s);
			if (!abbrs.containsKey(a)) {
				abbrs.put(a, new HashSet<String>());
			}
			abbrs.get(a).add(s);
		}
    }

    public boolean isUnique(String word) {
		String a = abbr(word);
		return !abbrs.containsKey(a) || (abbrs.get(a).contains(word) && abbrs.get(a).size() == 1);
    }

	private String abbr(String s) {
		if (s.length() <= 2) return s;
		return s.charAt(0) + String.valueOf(s.length() - 2) + s.charAt(s.length() - 1);
	}
}


// Your ValidWordAbbr object will be instantiated and called as such:
// ValidWordAbbr vwa = new ValidWordAbbr(dictionary);
// vwa.isUnique("Word");
// vwa.isUnique("anotherWord");
```
