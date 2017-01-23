# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,
Given [3,2,1,5,6,4] and k = 2, return 5.

Note: 
You may assume k is always valid, 1 ≤ k ≤ array's length.

## Solution 1. Heap

Use a heap to store up to k largest elements while iterating through the array.

Time: O(nlgk)

Space: O(k)

```java
public class Solution {
  public int findKthLargest(int[] nums, int k) {
    Queue<Integer> max = new PriorityQueue<Integer>();
    for (int n : nums) {
      if (max.size() < k) max.offer(n);
      else if (n > max.peek()) {
        max.poll();
        max.offer(n);
      }
    }
    return max.peek();
  }
}
```

## Solution 2. Linear selection

If we apply the same partitioning procedure as quicksort, we can find the kth element of an array in O(n) average time, or O(n^2) worst-case time if we are looking for the min while we always partition around the max element. If we partition around the median of medians, the algorithm is guaranteed to be O(n) on average.

Time: O(n) average, O(n^2) worst case

Space: O(1)

A recursive version:

```java
public class Solution {
  public int findKthLargest(int[] nums, int k) {
    return select(nums, 0, nums.length - 1, nums.length - k);
  }

  private int select(int[] nums, int start, int end, int i) {
    if (start == end) return nums[start];
    int j = partition(nums, start, end);
    if (i == j) return nums[j];
    if (i < j) return select(nums, start, j - 1, i);
    return select(nums, j + 1, end, i);
  }

  private int partition(int[] nums, int start, int end) {
    if (start > end) return -1;
    int i = start - 1;
    for (int j = start; j < end; j++) {
      if (nums[j] < nums[end]) {
        int t = nums[++i];
        nums[i] = nums[j];
        nums[j] = t;
      }
    }
    int t = nums[++i];
    nums[i] = nums[end];
    nums[end] = t;
    return i;
  }
}
```

A iterative version:

```java
public class Solution {
  public int findKthLargest(int[] nums, int k) {
    int low = 0, high = nums.length - 1;
    k = nums.length - k;
    while (low < high) {
      int pivot = partition(nums, low, high);
      if (pivot == k) return nums[k];
      if (pivot < k) low = pivot + 1;
      else high = pivot - 1;
    }
    return nums[low];
  }

  private int partition(int[] nums, int start, int end) {
    if (start > end) return -1;
    int i = start - 1;
    for (int j = start; j < end; j++) {
      if (nums[j] < nums[end]) {
        int t = nums[++i];
        nums[i] = nums[j];
        nums[j] = t;
      }
    }
    int t = nums[++i];
    nums[i] = nums[end];
    nums[end] = t;
    return i;
  }
}
```
