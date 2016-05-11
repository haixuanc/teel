# [77. Combinations](https://leetcode.com/problems/combinations/)

Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

For example,
If n = 4 and k = 2, a solution is:

[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

## Solution 1. Backtrace

```java
public class Solution {
    public List<List<Integer>> combine(int n, int k) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		dfs(res, new ArrayList<Integer>(), n, k);
		return res;
    }

	private void dfs(List<List<Integer>> res, List<Integer> path, int n, int k) {
		if (path.size() == k) {
			res.add(new ArrayList<Integer>(path));
			return;
		}
		for (int next = path.isEmpty() ? 1 : path.get(path.size() - 1) + 1; next <= n; next++) {
			path.add(next);
			dfs(res, path, n, k);
			path.remove(path.size() - 1);
		}
	}
}
```

## Solution 2. Recursion

- c(n, k) = null, if n < k or k < 0
- c(n, k) = [], if n >= k && k == 0
- c(n, k) = c(n - 1, k) + [for r in c(n - 1, k - 1) => r + n], otherwise

```java
public class Solution {
    public List<List<Integer>> combine(int n, int k) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		if (n < k || k < 0) return res;
		if (k == 0) {
			res.add(new ArrayList<Integer>());
			return res;
		}
		res = combine(n - 1, k - 1);
		for (List<Integer> r : res) {
			r.add(n);
		}
		res.addAll(combine(n - 1, k));
		return res;
	}
}
```
