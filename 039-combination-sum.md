# [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:

All numbers (including target) will be positive integers.
Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).
The solution set must not contain duplicate combinations.
For example, given candidate set 2,3,6,7 and target 7, 
A solution set is: 
[7] 
[2, 2, 3] 

## Solution. Backtrace/DFS

Backtrace is essentially a DFS algorithm. It relies on a shared data structrue to store the final solution. Because the data structured is shared by sub-routines recursively, we have to pop the top element off it when current sub-routine terminates to restore it to previous state.

Time: O(c(n, 1) + c(n, 2) + .. + c(n, n)), where c(n, m) = # of ways to draw m elements out of n elements.
Space: O(kn)

```java
public class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		Arrays.sort(candidates);
		find(res, new ArrayList<Integer>(), 0, candidates, target);
		return res;
    }

	private void find(List<List<Integer>> res, List<Integer> path, int index, int[] candidates, int target) {
		if (target < 0) return;
		if (target == 0) {
			res.add(new ArrayList<Integer>(path));
			return;
		}
		// We don't need to check duplications because the input array is
		// already sorted and we traverse through it in order.
		for (int i = index; i < candidates.length; i++) {
			path.add(candidates[i]);
			// NOTE: use i not i + 1 because we can reuse an element for
			// unlimited number of times.
			find(res, path, i, candidates, target - candidates[i]);
			path.remove(path.size() - 1);
		}
	}
}
```

### A minor revision - only continue iterating through the rest of the array if target is greater than or equal to current number

```java
public class Solution {
  public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    Arrays.sort(candidates);
    dfs(res, new ArrayList<Integer>(), candidates, target, 0);
    return res;
  }

  private void dfs(List<List<Integer>> res, List<Integer> path, int[] candidates, int target, int start) {
    if (target == 0) {
      res.add(new ArrayList<Integer>(path));
      return;
    }
    // To avoid adding duplicate combinations, for each call we only consider
    // sub-array of [start, ...]. I.e. we sort all valid combinations by their
    // first elements and iterate through them in order. Therefore the combinations
    // added are guaranteed to be unique.
    for (int i = start; i < candidates.length && candidates[i] <= target; i++) {
      path.add(candidates[i]);
      dfs(res, path, candidates, target - candidates[i], i);
      path.remove(path.size() - 1);
    }
  }
}
```
