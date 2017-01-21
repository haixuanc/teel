# [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example 1:

```
11110
11010
11000
00000
```

Answer: 1

Example 2:

```
11000
11000
00100
00011
```

Answer: 3

## Solution 1. DFS

Time: O(mn)

There will be at most mn islands. At each iteration, we will clear at least one island, so the time complexity will be O(mn)

Space: O(max(m, n)), maximum size of call stack

```java
public class Solution {
  public int numIslands(char[][] grid) {
    int islands = 0;
    for (int i = 0; i < grid.length; i++) {
      for (int j = 0; j < grid[i].length; j++) {
        if (grid[i][j] == '1') {
          islands++;
          clear(grid, i, j);
        }
      }
    }
    return islands;
  }

  private void clear(char[][] grid, int row, int col) {
    if (row < 0 || row >= grid.length || col < 0 || col >= grid[row].length || grid[row][col] == '0') return;
    grid[row][col] = '0';
    clear(grid, row, col + 1);
    clear(grid, row + 1, col);
    clear(grid, row, col - 1);
    clear(grid, row - 1, col);
  }
}
```

## Solution 2. Disjoint set/union-find

Time: O(mnlgmn), amortized cost for each union operation is O(lgmn) if we use ranked rooted trees to represent all sets

Space: O(mn)

```java
public class Solution {
  public int numIslands(char[][] grid) {
    if (grid.length == 0 || grid[0].length == 0) return 0;
    DisjointSet islands = new DisjointSet(grid.length * grid[0].length);
    for (int width = grid[0].length, i = 0; i < grid.length; i++) {
      for (int j = 0; j < grid[0].length; j++) {
        if (grid[i][j] == '1') {
          islands.makeSet(i * width + j);
          if (i > 0 && grid[i - 1][j] == '1') islands.union(i * width + j, (i - 1) * width + j);
          if (j > 0 && grid[i][j - 1] == '1') islands.union(i * width + j, i * width + j - 1);
        }
      }
    }
    return islands.count;
  }

  private class DisjointSet {
    int[] parents;
    int[] ranks;
    int count;

    DisjointSet(int size) {
      parents = new int[size];
      ranks = new int[size];
      count = 0;
    }

    int makeSet(int i) {
      count++;
      return parents[i] = i;
    }

    int find(int i) {
      if (parents[i] == i) return i;
      return parents[i] = find(parents[i]);
    }

    int union(int i, int j) {
      if (find(i) == find(j)) return find(i);
      count--;
      if (ranks[find(i)] == ranks[find(j)]) ranks[find(i)]++;
      return ranks[find(i)] > ranks[find(j)] ? (parents[find(j)] = find(i)) : (parents[find(i)] = find(j));
    }
  }
}
```
