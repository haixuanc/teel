# [190. Reverse Bits](https://leetcode.com/problems/reverse-bits/)

Reverse bits of a given 32 bits unsigned integer.

For example, given input 43261596 (represented in binary as 00000010100101000001111010011100), return 964176192 (represented in binary as 00111001011110000010100101000000).

Follow up:
If this function is called many times, how would you optimize it?

## Solution

Time: O(n)

Space: O(n)

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
		int reverse = 0;
		for (int i = 0; i < 32; i++) {
			reverse |= (n & 1) << (31 - i);
			n >>>= 1;
		}
		return reverse;
    }
}
```
