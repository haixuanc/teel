# [79. Word Search](https://leetcode.com/problems/word-search/)

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,
Given board =

[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
word = "ABCCED", -> returns true,
word = "SEE", -> returns true,
word = "ABCB", -> returns false.

## Solution. Backtrace

Time: O(nmk), matrix is n x m, string is of length of k

Space: O(nm), or O(k) if no auxiliary matrix

```java
public class Solution {
    public boolean exist(char[][] board, String word) {
		if (board.length == 0 || board[0].length == 0 || word.length() == 0) return false;
		boolean[][] visited = new boolean[board.length][board[0].length];
		for (int row = 0; row < board.length; row++) {
			for (int col = 0; col < board[0].length; col++) {
				if (dfs(board, visited, row, col, word, 0)) return true;
			}
		}
		return false;
    }

	private boolean dfs(char[][] board, boolean[][] visited, int row, int col, String word, int index) {
		if (index == word.length()) return true;
		if (row < 0 || row == board.length || col < 0 || col == board[0].length || visited[row][col] || board[row][col] != word.charAt(index)) return false;
		visited[row][col] = true;
		boolean found = dfs(board, visited, row - 1, col, word, index + 1) ||
						dfs(board, visited, row, col + 1, word, index + 1) ||
						dfs(board, visited, row + 1, col, word, index + 1) ||
						dfs(board, visited, row, col - 1, word, index + 1);
		visited[row][col] = false; // Reset visited nodes
		return found;
	}
}
```

## DFS vs. BFS

BFS is usually used to find the shortest path between two nodes in a graph. At each step, the algorithm will visit all child nodes of the current node. If we were to maintain all paths so far, the space complexity will grow exponentially. Although it is faster than DFS, but we won't use it to find a path satisfying certain critaria due to the heavy space complexity.

DFS only maintains a path to the current node. If the next child node does not satisfy the search criteria, we will backtrace to visit the next available child node. Therefore DFS is usually used as a subroutine for other search algorithms.
