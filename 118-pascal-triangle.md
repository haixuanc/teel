# [118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/description/)

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

## Solution 1

Time: O(n^2)

Space: O(n^2)

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> rows = new ArrayList<>();
        for (int i = 1; i <= numRows; i++) {
            Integer[] cur = new Integer[i];
            cur[0] = cur[cur.length - 1] = 1;
            for (int j = 1; j < cur.length - 1; j++) {
                cur[j] = rows.get(i - 2).get(j - 1) + rows.get(i - 2).get(j);
            }
            rows.add(Arrays.asList(cur));
        }
        return rows;
    }
}
```
