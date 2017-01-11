# [119. Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/)

Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return [1,3,3,1].

Note:
Could you optimize your algorithm to use only O(k) extra space?

## Solution 1. Traverse the array forward

f(0) = 1

f(k) = n0, n1, n2, …, nk-1, where n0 = 1, nk-1= 1

f(k + 1) = m0, m1, m2, …, mk, where mi = ni-1 + ni

Time: O(n^2)

Space: O(n)

```java
public class Solution {
  public List<Integer> getRow(int rowIndex) {
    int[] row = new int[rowIndex + 1];
    row[0] = 1;
    for (int i = 0, j = 1, prev = 0; j <= rowIndex && i <= j; i++) {
      row[i] += prev;
      prev = row[i] - prev;
      if (i == j) {
        i = -1; // NOTE: statement `i++` will be executed at the next iteration, so we will start from i = 0 again.
        j++;
      }
    }
    List<Integer> l = new ArrayList<Integer>();
    for (int n : row) l.add(n);
    return l;
  }
}
```

## Solution 2. Traverse the array backward

If we traverse the array forward, we have to use a variable to hold the old value for the last index because the last index has been updated.

To avoid that extra variable, we can traverse the array backward, i.e. a[n] = a[n] + a[n - 1]. Since a[n - 1] is not modified, we don't have to use one extra variable.

```java
public class Solution {
  public List<Integer> getRow(int rowIndex) {
    List<Integer> row = new ArrayList<Integer>();
    for (int i = 0; i <= rowIndex; i++) {
      row.add(1);
      for (int j = i - 1; j > 0; j--) {
        row.set(j, row.get(j) + row.get(j - 1));
      }
    }
    return row;
  }
}
```
