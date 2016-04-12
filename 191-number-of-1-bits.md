# [191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).

For example, the 32-bit integer ’11' has binary representation 00000000000000000000000000001011, so the function should return 3.

## Solution 1

Time: O(lgn)

Space: O(lgn)

```java
public class Solution {
  // you need to treat n as an unsigned value
  public int hammingWeight(int n) {
    int counter = 0;
    // CAUTION:
    // Don't use `n > 0` because n could be 1...
    while (n != 0) {
      counter += n & 1;
      n >>>= 1;
    }
    return counter;
  }
}
```

## Solution 2

Clear the last trailing 1 of a number: `n &= n -1`.

Assume n = x100..0, then n - 1= x011..0 => n & (n - 1) = x000..0

Time: O(# of 1 bits)
Space: O(lgn)

```java
public class Solution {
  // you need to treat n as an unsigned value
  public int hammingWeight(int n) {
    int counter = 0;
    while (n != 0) {
      n &= n - 1; // Remove the last trailing 1 of n
      counter++;
    }
    return counter;
  }
}
```
