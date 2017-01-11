# [414. Third Maximum Number](https://leetcode.com/problems/third-maximum-number/)

Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).

Example 1:
Input: [3, 2, 1]

Output: 1

Explanation: The third maximum is 1.
Example 2:
Input: [1, 2]

Output: 2

Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
Example 3:
Input: [2, 2, 3, 1]

Output: 1

Explanation: Note that the third maximum here means the third maximum distinct number.
Both numbers with value 2 are both considered as second maximum.

## Solution 1

The catch is we have to keep track of *the top n <= 3 **unique** numbers so far*.

Time: O(n)

Space: O(1)

```java
public class Solution {
  public int thirdMax(int[] nums) {
    int[] max = { Integer.MIN_VALUE, Integer.MIN_VALUE, Integer.MIN_VALUE };
    Set<Integer> uniques = new HashSet<Integer>();
    for (int n : nums) {
      if (uniques.contains(n)) continue;
      if (uniques.size() == 3 && n > max[0]) {
        uniques.remove(max[0]);
        uniques.add(n);
        max[0] = n;
        Arrays.sort(max);
      } else if (uniques.size() < 3) {
        uniques.add(n);
        max[0] = n;
        Arrays.sort(max);
      }
    }
    return uniques.size() == 3 ? max[0] : max[2];
  }
}
```
