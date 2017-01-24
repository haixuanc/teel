# [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

Given an array of n integers where n > 1, nums, return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Solve it without division and in O(n).

For example, given [1,2,3,4], return [24,12,8,6].

Follow up:
Could you solve it with constant space complexity? (Note: The output array does not count as extra space for the purpose of space complexity analysis.)

## Solution 1. Two pass scan

**Pass 1.**  product[i] = nums[0] x … x nums[i - 1]

**Pass 2.** products[i] x= nums[i + 1] x … x nums[n - 1]

Time: O(n)

Space: O(n)

```java
public class Solution {
  public int[] productExceptSelf(int[] nums) {
    if (nums.length == 0) return null;
    int[] products = new int[nums.length];
    products[0] = 1;
    for (int i = 1; i < nums.length; i++) {
      products[i] = products[i - 1] * nums[i - 1];
    }
    for (int i = products.length - 2, total = nums[i + 1]; i >= 0; i--) {
      products[i] *= total;
      total *= nums[i];
    }
    return products;
  }
}
```
