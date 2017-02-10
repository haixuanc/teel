# [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/)

Implement int sqrt(int x).

Compute and return the square root of x.

## Definition of sqaure root

r = sqrt(n), if (r + 1) ^ 2 > n && r ^ 2 <= n

It is easier to find the true estimate if we start with an overestimate.

## Solution 1. Naive one

Time: O(sqrt(n))

```java
public class Solution {
  public int mySqrt(int x) {
    int i = 1;
    while (i <= x / i) i++; // Use division to avoid integer overflow when multiple two integers
    return --i;
  }
}
```

## Solution 2. Binary search

The square root of n must exist in the sorted array [1, .., n]. We can apply binary search to find it.

Time: O(lgn)

```java
public class Solution {
    public int mySqrt(int x) {
		if (x == 0) return 0;
		int low = 1, high = x;
		while (low < high) {
			int mid = low + (high - low) / 2;
			// Use division to avoid integer overflow
			if (mid <= x / mid && (mid + 1) > x / (mid + 1)) return mid;
			if (mid < x / mid) low = mid + 1;
			else high = mid - 1;
		}
		return low;
    }
}
```

## Solution 3. Newton's method

Algebaric meaning:
If current value `x` is an overestimate, then `s / x` will be an underestimate. So the average of them will be a better estimate.

Geomeric meaning:
The x intercept x2 of the tangent line of current guess x1 will be a better estimate.

f(x) = x^2 - s, we want to find an x' such that f(x') = 0.
- We start with a close guess x1, the tangent at x1 on f(x) is f'(x1).
- The x intercept x2 of the tangent line is defined as: (f(x1) - 0) / (x1 - x2) = f'(x1)
- After solving the equation, we get: x2 = (x1 + s / x1) / 2
- We apply this calculation iteratively until f(xn) no longer converges to zero.

Time: O(lgn)

```java
public class Solution {
  public int mySqrt(int x) {
    if (x == 0) return 0;
    // NOTE: zero cannot be a square root
    int root = x;
    while (root > x / root) {
      // Use `lower + (upper - lower) / 2` to avoid integer overflow when adding two large integers
      root = x / root + ((root - x / root) >> 1);
    }
    return root;
  }
}
```
