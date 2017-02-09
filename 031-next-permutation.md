# [31. Next Permutation](https://leetcode.com/problems/next-permutation/)

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

## Solution

- If nums is in descending order, it is the largest permuation. The next permutation is the minimum permutation which is the lexical order of characters in nums.
- Scan nums from right to left, least-significant digit to most-significant digit, find the first index i such that `nums[i] < nums[i + 1]`. Because `nums[i] < nums[i + 1]`, `nums[i:]` is not the largest permutation for segment `[i:]`. We can find the next permutation for segment `[i:]`, and the overall permutation will be the next permutation for the entire array.
  - The next permutation for segment `[i:]` is `min(nums[i+1:]) + minPermutation(nums[i+1:])`.
  - Find the smallest element in `nums[i+1:]`, say `nums[j]`, that is greater than `nums[i]`.
  - Swap `nums[i]` and `nums[j]`.
  - `nums[i+1:]` is still in descending order.
  - Reverse `nums[i+1:]`.

```java
public class Solution {
  public void nextPermutation(int[] nums) {
    if (nums.length <= 1) return;
    int i = nums.length - 1;
    while (i > 0 && nums[i - 1] >= nums[i]) i--;
    if (i > 0) {
      int j = nums.length - 1;
      while (j >= i && nums[j] <= nums[i - 1]) j--;
      swap(nums, i - 1, j);
    }
    for (int j = nums.length - 1; i < j; ) {
      swap(nums, i++, j--);
    }
  }

  private void swap(int[] nums, int i, int j) {
    int t = nums[i];
    nums[i] = nums[j];
    nums[j] = t;
  }
}
```
