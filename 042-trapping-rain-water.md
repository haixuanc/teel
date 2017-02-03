# [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example, 
Given [0,1,0,2,1,0,1,3,2,1,2,1], return 6.

## Solution 1. Two pointers

If a slot i can trap some water it must be bounded by two other slots j and k, where j < i < k, and h[i] < h[j] && h[i] < h[k]. The amount of water that i can trap is min(h[j], h[k]) - h[i]. So the question is how to determine j and k. It can be easily proved that h[j] = max(h[..., i)) and h[k] = max(h(i, …]).

Time: O(n)

Space: O(1)

```java
public class Solution {
  public int trap(int[] height) {
    int total = 0;
    for (int left = 0, right = height.length - 1, maxLeft = 0, maxRight = 0; left < right; ) {
      if (height[left] <= height[right]) {
        maxLeft = Math.max(maxLeft, height[left]);
        total += maxLeft - height[left++];
      } else {
        maxRight = Math.max(maxRight, height[right]);
        total += maxRight - height[right--];
      }
    }
    return total;
  }
}
```

## Solution 2. Stack

In order to determine how much water a slot can trap, we must know both of its left and right bounds.

h(i, j) = how much **extra** water bin[i, j] can trap compared to bin[i + 1, j]

The total water bin[i, j] can trap = sum(h(k, j) for k < j)

**NOTE:** if height[j + 1] >= height[j], bin(j + 1, j) won't contribute any extra water. If height[i] > height[j], for any k < i such that height[k] <= height[i], bin(k, j) won't contribute any extra water either. Only k < i and height[k] > height[i] can contribute extra water. So we need to track heights in descending order.

Time: O(n)

Space: O(n)

```java
public class Solution {
  public int trap(int[] height) {
    if (height.length <= 2) return 0;
    int total = 0;
    Deque<Integer> downslope = new ArrayDeque<Integer>();
    for (int i = 0; i < height.length; ) {
      if (downslope.isEmpty() || height[i] <= height[downslope.peekFirst()]) downslope.addFirst(i++);
      else {
        int bottom = height[downslope.removeFirst()];
        total += downslope.isEmpty() ? 0 : (Math.min(height[downslope.peekFirst()], height[i]) - bottom) * (i - downslope.peekFirst() - 1);
      }
    }
    return total;
  }
}
```
