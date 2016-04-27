# [344. Reverse String](https://leetcode.com/problems/reverse-string/)

Write a function that takes a string as input and returns the string reversed.

Example:
Given s = "hello", return "olleh".

## Solution

```java
public class Solution {
    public String reverseString(String s) {
		char[] str = s.toCharArray();
		for (int i = 0, j = str.length - 1; i < j; i++, j--) {
			char c = str[i];
			str[i] = str[j];
			str[j] = c;
		}
		return new String(str);
    }
}
```
