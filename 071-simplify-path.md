# [71. Simplify Path](https://leetcode.com/problems/simplify-path/)

Given an absolute path for a file (Unix-style), simplify it.

For example,
path = "/home/", => "/home"
path = "/a/./b/../../c/", => "/c"

Corner Cases:
Did you consider the case where path = "/../"?
In this case, you should return "/".
Another corner case is the path might contain multiple slashes '/' together, such as "/home//foo/".
In this case, you should ignore redundant slashes and return "/home/foo".

## Solution. Stack

Usually, stack is the most natural data structure to use to memorize things.

```java
public class Solution {
    public String simplifyPath(String path) {
		Deque<String> visited = new ArrayDeque<String>();
		for (int i = 0; i < path.length(); i++) {
			if (path.charAt(i) == '/') continue;
			int j = i;
			while (j < path.length() && path.charAt(j) != '/') j++;
			String cur = path.substring(i, j);
			i = j;
			if (cur.equals(".")) continue;
			if (cur.equals("..") && !visited.isEmpty()) visited.pop();
			else if (!cur.equals("..")) visited.push(cur);
		}
		if (visited.isEmpty()) return "/";
		StringBuilder p = new StringBuilder();
		while (!visited.isEmpty()) {
			p.insert(0, '/' + visited.pop());
		}
		return p.toString();
    }
}
```
