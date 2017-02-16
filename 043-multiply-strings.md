# [43. Multiply Strings](https://leetcode.com/problems/multiply-strings/)

Given two numbers represented as strings, return multiplication of the numbers as a string.

Note:
The numbers can be arbitrarily large and are non-negative.
Converting the input string to integer is NOT allowed.
You should NOT use internal library such as BigInteger.

## Solution. Sum of single-digit multiplications

[A detailed explanation with graphic](https://leetcode.com/discuss/71593/easiest-java-solution-with-graph-explanation)

```
  ab
x cd
____
  bd
 ad
 bc
ac
```

```java
public class Solution {
    public String multiply(String num1, String num2) {
		int[] res = new int[num1.length() + num2.length()];
		for (int i = num1.length() - 1; i >= 0; i--) {
			for (int j = num2.length() - 1; j >= 0; j--) {
				int n = (num1.charAt(i) - '0') * (num2.charAt(j) - '0') + res[i + j + 1];
				res[i + j + 1] = n % 10;
				res[i + j] += n / 10;
			}
		}
		StringBuilder sb = new StringBuilder();
		int i = 0;
		// Skip leading zeros excluding the last one, e.g. 0 * xxx = 000 => 0
		while (i < res.length - 1 && res[i] == 0) i++;
		while (i < res.length) sb.append(res[i++]);
		return sb.toString();
    }
}
```

```java
public class Solution {
  public String multiply(String num1, String num2) {
    if (num1.length() == 0 || num2.length() == 0 || num1.equals("0") || num2.equals("0")) return "0";
    int[] product = new int[num1.length() + num2.length()];
    for (int i = num1.length() - 1; i >= 0; i--) {
      for (int j = num2.length() - 1; j >= 0; j--) {
        product[i + j + 1] += (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
        product[i + j] += product[i + j + 1] / 10;
        product[i + j + 1] %= 10;
      }
    }
    StringBuilder p = new StringBuilder();
    if (product[0] != 0) p.append(product[0]);
    for (int i = 1; i < product.length; i++) p.append(product[i]);
    return p.toString();
  }
}
```
