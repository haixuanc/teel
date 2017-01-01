# [27. Remove Element](https://leetcode.com/problems/remove-element/)

Given an array and a value, remove all instances of that value in place and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example:
Given input array nums = [3,2,2,3], val = 3

Your function should return length = 2, with the first two elements of nums being 2.

Hint:

1. Try two pointers.
2. Did you use the property of "the order of elements can be changed"?
3. What happens when the elements to remove are rare?

## Solution 1. Copy legal elements

Time: O(n)

For every illegal element all elements after it will be moved in order to replace it. So time cost will be close to O(n) if illegal elements appear early in the array even if they are rare.

Space: O(1)

```java
public class Solution {
  public int removeElement(int[] nums, int val) {
    int i = -1;
    for (int j = 0; j < nums.length; ++j) {
      if (nums[j] != val) nums[++i] = nums[j];
    }
    return i + 1;
  }
}
```

## Solution 2. Remove illegal elements

Time: O(n)

Space: O(1)

If we replace every illegal element with the last element, only one element needs to be moved. So if illegal elements are rare, time cost will be close to O(1).

The idea is to shrink the array until it only contains legal elements.

```java
public class Solution {
  public int removeElement(int[] nums, int val) {
    int i = nums.length - 1;
    for (int j = 0; j <= i; ) {
      if (nums[j] == val) nums[j] = nums[i--];
      else ++j;
    }
    return i + 1;
  }
}
```
