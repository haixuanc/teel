# [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Example 1:
Input: [7, 1, 5, 3, 6, 4]
Output: 5

max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
Example 2:
Input: [7, 6, 4, 3, 1]
Output: 0

In this case, no transaction is done, i.e. max profit = 0.

## Solution 1. DP

f(n) = the max profit that could be earned in the first n days

g(i) = the max profit earned if we sell stock on day i

f(n) = max(g(i) for i <= n)

g(i) = pi - min(pj for j < i)

Time: O(n)

Space: O(1)

```java
public class Solution {
  public int maxProfit(int[] prices) {
    if (prices.length == 0) return 0;
    int max = 0;
    for (int min = prices[0], i = 1; i < prices.length; i++) {
      max = Math.max(max, prices[i] - min);
      min = Math.min(min, prices[i]);
    }
    return max;
  }
}
```

## Solution 2. Kadane's algorithm for maximum subarray problem

This problem can be generalized as: finding a subarray of an integer array with the maximum sum.

Kadane's algorithm (a simple dynamic programming solution):

n[i] = the ith element in the array

f(i) = sum of maximum subarray ending at index i

f(0) = 0

f(i) = max(n[i], f(i - 1) + n[i])

Time: O(n)

Space: O(1)

We can convert the stock price array into an array of difference prices, i.e.:

- p[0] = 0
- p[i] = p[i] - p[i - 1], for i > 0

Finding a pair of indices with the greatest price difference is equivalent as finding a subarray with the maximum sum.

```java
public class Solution {
  public int maxProfit(int[] prices) {
    int maxEndingHere = 0;
    int maxSoFar = 0;
    for (int i = 1; i < prices.length; i++) {
      maxEndingHere = Math.max(0, maxEndingHere += prices[i] - prices[i -1]);
      maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
  }
}
```
