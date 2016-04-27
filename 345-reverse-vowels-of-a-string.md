# [345. Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)

Write a function that takes a string as input and reverse only the vowels of a string.

Example 1:
Given s = "hello", return "holle".

Example 2:
Given s = "leetcode", return "leotcede".

## Solution. Two pointers

```java
public class Solution {
    public String reverseVowels(String s) {
		Set<Character> vowels = new HashSet<Character>();
		vowels.add('a'); vowels.add('A');
		vowels.add('e'); vowels.add('E');
		vowels.add('i'); vowels.add('I');
		vowels.add('o'); vowels.add('O');
		vowels.add('u'); vowels.add('U');
		char[] str = s.toCharArray();
		for (int i = 0, j = str.length - 1; i < j; ) {
			if (vowels.contains(str[i]) && vowels.contains(str[j])) {
				char c = str[i];
				str[i] = str[j];
				str[j] = c;
				i++;
				j--;
				continue;
			}
			if (!vowels.contains(str[i])) {
				i++;
			}
			if (!vowels.contains(str[j])) {
				j--;
			}
		}
		return new String(str);
    }
}
```
