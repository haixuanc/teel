# [136. Single Number](https://leetcode.com/problems/single-number/description/)

Given an array of integers, every element appears twice except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

## Solution 1. XOR bit manipulation

Time: O(n)

Space: O(1)

```java
class Solution {
    public int singleNumber(int[] nums) {
        int single = 0;
        for (int n : nums) {
            single ^= n;
        }
        return single;
    }
}
```
