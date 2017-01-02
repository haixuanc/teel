# [88. Merge Sorted Arrays](https://leetcode.com/problems/merge-sorted-array/)

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

## Solution 1

One optimization: once all the numbers of `nums2` have been copied into `nums1`, if `nums1` has any unexamined numbers those numbers are already sorted. So the loop can terminate once all of `nums2` have been copied over.

Time: O(m + n)

Space: O(1)

```java
public class Solution {
  public void merge(int[] nums1, int m, int[] nums2, int n) {
    for (int i = (m--) + (n--) - 1; n >= 0; ) {
      nums1[i--] = m < 0 || nums2[n] >= nums1[m] ? nums2[n--] : nums1[m--];
    }
  }
}
```
