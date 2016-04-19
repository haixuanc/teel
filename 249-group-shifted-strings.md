# [249. Group Shifted Strings](https://leetcode.com/problems/group-shifted-strings/)

Given a string, we can "shift" each of its letter to its successive letter, for example: "abc" -> "bcd". We can keep "shifting" which forms the sequence:

"abc" -> "bcd" -> ... -> "xyz"
Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

For example, given: ["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"], 
Return:

[
  ["abc","bcd","xyz"],
  ["az","ba"],
  ["acef"],
  ["a","z"]
]

Note: For the return value, each inner list's elements must follow the lexicographic order.

## Solution

```java
public class Solution {
    public List<List<String>> groupStrings(String[] strings) {
		Map<String, List<String>> groups = new HashMap<String, List<String>>();
		for (String s : strings) {
			String k = key(s);
			if (!groups.containsKey(k)) {
				groups.put(k, new ArrayList<String>());
			}
			groups.get(k).add(s);
		}
		for (String k : groups.keySet()) {
			Collections.sort(groups.get(k));
		}
		return new ArrayList<List<String>>(groups.values());
    }

	private String key(String s) {
		if (s.length() == 0) return "";
		StringBuilder k = new StringBuilder();
		k.append("0");
		for (int i = 1; i < s.length(); i++) {
			int d = s.charAt(i) - s.charAt(0);
			k.append("," + (d < 0 ? d + 26 : d));
		}
		return k.toString();
	}
}
```
