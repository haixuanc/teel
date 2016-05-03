# [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

Each number in C may only be used once in the combination.

Note:
All numbers (including target) will be positive integers.
Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).
The solution set must not contain duplicate combinations.
For example, given candidate set 10,1,2,7,6,1,5 and target 8, 
A solution set is: 
[1, 7] 
[1, 2, 5] 
[2, 6] 
[1, 1, 6] 

## Solution. Backtrace/DFS

Time: O(n! + (n-1)! + .. + 1)

```java
public class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		Arrays.sort(candidates);
		dfs(res, new ArrayList<Integer>(), 0, candidates, target);
		return res;
    }

	private void dfs(List<List<Integer>> res, List<Integer> path, int index, int[] candidates, int target) {
		if (target < 0) return;
		if (target == 0) {
			res.add(new ArrayList<Integer>(path));
			return;
		}
		for (int i = index; i < candidates.length; i++) {
			// Skip the next same element to avoid duplicated solutions
			//
			// Loop invariant:
			// At step i, all solutions with leading elements of a[:i] have
			// already been explored because in DFS a node is marked visited
			// when all of its children have been marked visited.
			if (i != index && candidates[i] == candidates[i - 1]) continue;
			path.add(candidates[i]);
			dfs(res, path, i + 1, candidates, target - candidates[i]);
			path.remove(path.size() - 1);
		}
	}
}
```

## How backtrace works

Backtrace is the technique of using a shared data structure to store the final solution when traversing a graph in a DFS manner.
Since the data structure is shared by sub-routines, we need to make a copy of it once we find a solution. In addition, we need to remove the last element from the data structure when a sub-routine terminates. So the shared data structure is used like a last-in-first-out stack.
