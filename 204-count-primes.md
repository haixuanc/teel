# [204. Count Primes](https://leetcode.com/problems/count-primes/)

Description:

Count the number of prime numbers less than a non-negative number, n.

## Solution 1. Sieve of Eratosthenes

[Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#Algorithm_complexity)

The main idea is to iterate through each number in ascending order and mark off the following numbers which are the multiples of the current number. In the next iteration, we skip all following numbers until we meet a number that has not been marked off. That number is the next prime number. After we have iterated through all numbers, the numbers left will be the prime numbers in the range.

There are some properties of the Sieve of Eratosthenes that we can utilize to optimize our algorithm:

- Loop invariant: for any number i, all numbers in the range which are the multiples for any number j in [2, i) have already been marked off in previous iterations.
- Since if i is not a prime number, it must be the multiple for some number j in the range [2, i). Conversely if i is not marked off, i must be a prime number.
- When marking off the multiples for i, we don't have to start from i, 2i, 3i, …. Instead, we only need to start from i^2 because for k < i, k * i has already been marked off in previous iterations.
- We don't have to iterate through all numbers to mark off non-primes. For any non-prime i <= n, i must have some factors j <= sqrt(i) <= sqrt(n). So we only need to scan until sqrt(n), and by that time we will have marked off all non-primes in the range.

A rough time analysis: O(n ^ 1.5)

For each number i, we will probe its prime factors in the range of [2, sqrt(i)]. Since i <= n, we will mark off a non-prime number at most sqrt(n) times. In total, for n numbers, each number will be marked off at most sqrt(n) times. The total time cost is O(n * sqrt(n)) = O(n ^ 1.5)

```java
public class Solution {
  public int countPrimes(int n) {
    boolean[] isPrime = new boolean[n];
    for (int i = 2; i < isPrime.length; i++) isPrime[i] = true;
    for (int i = 2; i * i < n; i++) {
      if (!isPrime[i]) continue;
      for (int j = i * i; j < n; j += i) {
        isPrime[j] = false;
      }
    }
    int count = 0;
    for (int i = 2; i < n; i++) {
      if (isPrime[i]) count++;
    }
    return count;
  }
}
```
