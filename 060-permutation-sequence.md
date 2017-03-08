# [60. Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)

The set [1,2,3,…,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):

"123"
"132"
"213"
"231"
"312"
"321"
Given n and k, return the kth permutation sequence.

Note: Given n will be between 1 and 9 inclusive.

## Solution 1. Recursively generate the next permutation

[31. Next permutation](https://leetcode.com/problems/next-permutation/)

## Solution 2. Generate Kth permutation directly

The fact about Kth permutation is that:
- ixx..x = (i - 1) * (n-1)! th permutation if i > 1
- ixx..x = j th permutation where j = [1, (n - 1)!] if i == 1

At each iteration:
- digit(i) = (k / (n - 1 - i)!) th of the remaining digits
- k %= (n - 1 - i)!

Time: O(n)

Space: O(n)

```java
// NOTE: factorial number grows very fast so integer will overflow easily.
// The problem assumes n = [1, 9], 9! is less than 2^31 - 1, so we don't have
// to worry about integer overflow.
public class Solution {
    public String getPermutation(int n, int k) {
		List<Integer> digits = new LinkedList<Integer>();
		int count = 1;
		for (int i = 1; i <= n; i++) {
			digits.add(i);
			count *= i;
		}
		--k; // Make k zero-based
		StringBuilder p = new StringBuilder();
		while (n > 0) {
			count /= n--;
			p.append(digits.remove(k / count));
			k %= count;
		}
		return p.toString();
    }
}
```
