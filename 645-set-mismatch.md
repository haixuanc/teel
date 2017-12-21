# [645. Set Mismatch](https://leetcode.com/problems/set-mismatch/description/)

The set S originally contains numbers from 1 to n. But unfortunately, due to the data error, one of the numbers in the set got duplicated to another number in the set, which results in repetition of one number and loss of another number.

Given an array nums representing the data status of this set after the error. Your task is to firstly find the number occurs twice and then find the number that is missing. Return them in the form of an array.

Example 1:

```
Input: nums = [1,2,2,4]
Output: [2,3]
```

Note:

```
1. The given array size will in the range [2, 10000].
2. The given array's numbers won't have any order.
```

## Solution 1. Sort first and linear scan afterwards

Time: O(nlgn)

Space: O(1)

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        if (nums.length == 0) return new int[] {};
        Arrays.sort(nums);
        int[] res = new int[] { -1, -1 };
        // The array must start with 1, otherwise 1 is missing
        if (nums[0] != 1) res[1] = 1;
        for (int i = 1; i < nums.length; i++) {
            // Duplicate found
            if (nums[i] == nums[i - 1]) {
                res[0] = nums[i];
            }
            // Not duplicate but the next number is skipped
            else if (nums[i] != nums[i - 1] + 1) {
                res[1] = nums[i - 1] + 1;
            }
        }
        // If missing number is not found so far, the number
        // after upper bound will be the missing one
        if (res[1] == -1) res[1] = nums[nums.length - 1] + 1;
        return res;
    }
}
```

## Solution 2. One pass XOR

Let's assume:

- S = 1 ^ 2 ^ 3 ^ ... ^ (n - 1)
- S' = 1 ^ 2 ^ ... ^ x ^ x ^ ... ^ (y - 1) ^ (y + 1) ^ ... ^ (n - 1)

Since x ^ x = 0, duplicating x cancels itself, S = S' ^ x ^ y => y = S ^ S' ^ x

We can easily find the duplicate number x by tracking the appearance of each number in the array using for example a hash map or array in this case for more efficient memory use.

The XOR operation is not necessary, we can use summation or mutiplication as well. For example:

- S' = S + x - y
- S' = S / x * y

But with XOR we don't have to worry about overflow, and the operation is faster.

Time: O(n)

Space: O(n)

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int[] res = new int[2];
        boolean[] seen = new boolean[nums.length];
        for (int i = 0; i < nums.length; i++) {
            if (seen[nums[i] - 1]) {
                res[0] = nums[i];
            } else {
                seen[nums[i] - 1] = true;
            }
            res[1] ^= nums[i] ^ (i + 1);
        }
        res[1] ^= res[0];
        return res;
    }
}
```

## Solution 3. Two pass with no extra storage

Trick: utilize the original array as a hash table to mark numbers visited.

Time: O(n)

Space: O(1)

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int[] res = new int[2];
        // Use original array as a hash table to mark visited numbers
        // nums[n - 1] > 0 if n is not visited
        // nums[n - 1] < 0 if n is visited
       for (int n : nums) {
            n = Math.abs(n);
            if (nums[n - 1] < 0) {
                res[0] = n;
            } else {
                nums[n - 1] *= -1;
            }
        }
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                res[1] = i + 1; // i + 1 is the missing number not the respective value
            } else {
                nums[i] *= -1; // Restore to its original value
            }
        }
        return res;
    }
}
```
