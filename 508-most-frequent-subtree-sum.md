# [508. Most Frequent Subtree Sum](https://leetcode.com/problems/most-frequent-subtree-sum/)

Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.

Examples 1

Input:

```
 5
  /  \
  2   -3
```

return [2, -3, 4], since all the values happen only once, return all of them in any order.

Examples 2

Input:

```
  5
   /  \
   2   -5
```

return [2], since 2 happens twice, however -5 only occur once.

Note: You may assume the sum of values in any subtree is in the range of 32-bit signed integer.

## Solution 1. One pass + Frequency maps

Time: O(n)

Space: O(n)

```java
/**
 * Definition for a binary tree node.
 *  * public class TreeNode {
 *   *     int val;
 *    *     TreeNode left;
 *     *     TreeNode right;
 *      *     TreeNode(int x) { val = x; }
 */
public class Solution {
  private int maxFreq = 0;

  public int[] findFrequentTreeSum(TreeNode root) {
    Map<Integer, Integer> freqs = new HashMap<Integer, Integer>();
    Map<Integer, Set<Integer>> groups = new HashMap<Integer, Set<Integer>>();
    groups.put(0, new HashSet<Integer>());
    dfs(root, freqs, groups);
    int[] res = new int[groups.get(maxFreq).size()];
    int i = 0;
    for (int s : groups.get(maxFreq)) res[i++] = s;
    return res;
  }

  private int dfs(TreeNode root, Map<Integer, Integer> freqs, Map<Integer, Set<Integer>> groups) {
    if (root == null) return 0;
    int sum = dfs(root.left, freqs, groups) + dfs(root.right, freqs, groups) + root.val;
    freqs.put(sum, !freqs.containsKey(sum) ? 1 : freqs.get(sum) + 1);
    maxFreq = Math.max(maxFreq, freqs.get(sum));
    if (!groups.containsKey(maxFreq)) groups.put(maxFreq, new HashSet<Integer>());
    if (freqs.get(sum) == maxFreq) groups.get(maxFreq).add(sum);
    return sum;
  }
}
```
