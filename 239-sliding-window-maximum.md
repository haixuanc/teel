# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

For example,
Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.

```
   Window position                Max
   ---------------               -----
   [1  3  -1] -3  5  3  6  7       3
   1 [3  -1  -3] 5  3  6  7       3
   1  3 [-1  -3  5] 3  6  7       5
   1  3  -1 [-3  5  3] 6  7       5
   1  3  -1  -3 [5  3  6] 7       6
   1  3  -1  -3  5 [3  6  7]      7
```

Therefore, return the max sliding window as [3,3,5,5,6,7].

Note: 
You may assume k is always valid, ie: 1 ≤ k ≤ input array's size for non-empty array.

Follow up:
Could you solve it in linear time?

Hint:

1. How about using a data structure such as deque (double-ended queue)?
2. The queue size need not be the same as the window’s size.
3. Remove redundant elements and the queue should store only elements that need to be considered.

## Solution 1. Heap

Time: O(nlgk)

Space: O(k)

```java
public class Solution {
  public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums.length == 0 || k == 0) return new int[0];
    int[] res = new int[nums.length - k + 1];
    Queue<Integer> seen = new PriorityQueue<Integer>(res.length, new Comparator<Integer>() {
        @Override
        public int compare(Integer a, Integer b) {
        return a == b ? 0 : (a < b ? 1 : -1);
        }
        });
    for (int i = 0; i < k; i++) seen.add(nums[i]);
    for (int i = 0; i < res.length; i++) {
      res[i] = seen.peek();
      if (i == res.length - 1) break;
      seen.remove(nums[i]);
      seen.add(nums[i + k]);
    }
    return res;
  }
}
```

## Solution 2. Descending double-ended queue

The idea is to maintain a list of selective elements in descending order in the current window. Remove the oldest element if it is out of the current window. 

Loop invariant: the queue only contains maximum element in the current window and elements that will possible be the maximum element in the next window.

For the ith element, we will first remove element out of the current window. Then for each element in the queue if it is smaller than the current element we will remove it too because it won't be the maximum element in the current window or future windows.

Since we always add new element to the end of the queue and remove any preceding elements that is smaller than the new one, so the first element element of the queue will be the maximum.

Time: O(n)

Space: O(k)

```java
public class Solution {
  public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums.length == 0 || k == 0) return new int[0];
    int[] res = new int[nums.length - k + 1];
    Deque<Integer> candidates = new ArrayDeque<Integer>();
    for (int i = 0, j = 0; i < nums.length; i++) {
      if (!candidates.isEmpty() && candidates.peekFirst() < i + 1 - k) candidates.removeFirst();
      while (!candidates.isEmpty() && nums[candidates.peekLast()] <= nums[i]) candidates.removeLast();
      candidates.addLast(i);
      if (i >= k - 1) res[j++] = nums[candidates.peekFirst()];
    }
    return res;
  }
}
```
