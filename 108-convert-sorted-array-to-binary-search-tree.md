# [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

## Solution 1. Recursion

Time: O(n)

Space: O(n)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return toBst(nums, 0, nums.length - 1);
    }

    private TreeNode toBst(int[] nums, int left, int right) {
        if (left > right) return null;
        int mid = left + (right - left) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = toBst(nums, left, mid - 1);
        root.right = toBst(nums, mid + 1, right);
        return root;
    }
}
```
