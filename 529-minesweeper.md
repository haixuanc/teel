# [529. Minesweeper](https://leetcode.com/problems/minesweeper/?tab=Description)

Checkout leetcode for problem details.

## DFS

Time: O(mn)

Space: O(m + n)

```java
public class Solution {
  public char[][] updateBoard(char[][] board, int[] click) {
    dfs(board, click[0], click[1]);
    return board;
  }

  // This helper method is not necessary if we apply recursion on updateBoard() directly
  private boolean dfs(char[][] board, int x, int y) {
    if (x < 0 || x >= board.length || y < 0 || y >= board[x].length) return true;
    if (board[x][y] != 'M' && board[x][y] != 'E') return true;
    if (board[x][y] == 'M') {
      board[x][y] = 'X';
      return false;
    }
    int adjMines = count(board, x, y);
    if (adjMines > 0) {
      board[x][y] = (char) ('0' + adjMines);
      return true;
    }
    board[x][y] = 'B';
    return dfs(board, x - 1, y) && dfs(board, x - 1, y + 1) && dfs(board, x, y + 1) && dfs(board, x + 1, y + 1) && dfs(board, x + 1, y) && dfs(board, x + 1, y - 1) && dfs(board, x, y - 1) && dfs(board, x - 1, y - 1);
  }

  private int count(char[][] board, int x, int y) {
    int n = 0;
    n += x - 1 >= 0 && board[x - 1][y] == 'M' ? 1 : 0;
    n += x - 1 >= 0 && y + 1 < board[x - 1].length && board[x - 1][y + 1] == 'M' ? 1 : 0;
    n += y + 1 < board[x].length && board[x][y + 1] == 'M' ? 1 : 0;
    n += x + 1 < board.length && y + 1 < board[x + 1].length && board[x + 1][y + 1] == 'M' ? 1 : 0;
    n += x + 1 < board.length && board[x + 1][y] == 'M' ? 1 : 0;
    n += x + 1 < board.length && y - 1 >= 0 && board[x + 1][y - 1] == 'M' ? 1 : 0;
    n += y - 1 >= 0 && board[x][y - 1] == 'M' ? 1 : 0;
    n += x - 1 >= 0 && y - 1 >= 0 && board[x - 1][y - 1] == 'M' ? 1 : 0;
    return n;
  }
}
```
