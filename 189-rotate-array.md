# [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

Rotate an array of n elements to the right by k steps.

For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].

Note:
Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.

## Solution 1. Naive O(kn) time complexity

Shift the entire array to the right for k times. Each time all elements of the array are shifted, therefore in total kn shifts.

## Solution 2. Use a buffer to hold the k trailing elements

Time: O(n)

Space: O(k)

```java
public class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        if (k == 0) return;
        int[] buffer = new int[k];
        for (int i = 0; i < k; i++) {
            buffer[i] = nums[nums.length - k + i];
        }
        for (int i = nums.length - k - 1; i >= 0; i--) {
            nums[i + k] = nums[i];
        }
        for (int i = 0; i < k; i++) {
            nums[i] = buffer[i];
        }
    }
}
```

## Solution 3. Modification of square rotation

Think of the array as concatenation of sides of length of k, with last side of length of [0, k). The problem is converted to rotating a "poly-square".

Similar to square rotation problem, we can do rotation in place if we rotate a[i], a[i + k], a[i + 2k], … at ith step. The trick situation happens when i + jk >= |a| - 1 and we have to wrap it around. Say p = (i + jk) % |a|. It is possible p = i. To avoid repeating the previous rotation, we need to increment i to i + 1 to start the next rotation.

When to stop? Notice each element in the array is shifted exactly once. We can keep track of number of elements shifted so far, and stop when it equals the size of the array.

Time: O(n)

Space: O(1)

```java
public class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        if (k == 0) return;
        int rotated = 0;
        int start = 0;
        int i = k;
        int prev = nums[0];
        while (rotated++ < nums.length) {
            int cur = nums[i];
            nums[i] = prev;
            if (i == start) {
                i = ++start;
                prev = nums[i];
            } else {
                prev = cur;
            }
            i += k;
            i %= nums.length;
        }
    }
}
```

## Solution 4. Reverse segments of the array

Divide the array into two segments: a = m + n, where |n| = k. Define a' as the reverse of a.

1. a => a' = n' + m'
2. n' => n, m' => m => n + m

Time: O(n)

Space:O(1)

```java
public class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        if (k == 0) return;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }
    
    private void reverse(int[] nums, int start, int end) {
        for ( ; start < end; start++, end--) {
            int tmp = nums[start];
            nums[start] = nums[end];
            nums[end] = tmp;
        }
    }
}
```
