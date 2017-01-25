# [89. Gray Code](https://leetcode.com/problems/gray-code/)

The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

For example, given n = 2, return [0,1,3,2]. Its gray code sequence is:

00 - 0
01 - 1
11 - 3
10 - 2
Note:
For a given n, a gray code sequence is not uniquely defined.

For example, [0,2,3,1] is also a valid gray code sequence according to the above definition.

For now, the judge is able to judge based on one instance of gray code sequence. Sorry about that.

## Solution 1. Recursion

When a problem is to find the solution for n, it is good indication that the problem can be solved recursively. Define the recursion and find the solution for the base case to solve the problem for n.

Time: O(2^n)

Space: O(2^n)

f(0) = [0]

f(n) = [for v in f(n - 1) => 0v] + [for v in reverse(f(n - 1)) => 1v]

or

f(n) = [for v in f(n - 1) => v0] + [for v in reverse(f(n - 1)) => v1]

```java
public class Solution {
  public List<Integer> grayCode(int n) {
    List<Integer> codes = new ArrayList<Integer>();
    codes.add(0);
    for (int i = 1; n-- > 0; i <<= 1) {
      for (int last = codes.size(); --last >= 0; codes.add(codes.get(last) + i)) {};
    }
    return codes;
  }
}
```
