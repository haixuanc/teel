# [292. Nim Game](https://leetcode.com/problems/nim-game/)

You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.

Hint:
If there are 5 stones in the heap, could you figure out a way to remove the stones such that you will always be the winner?

## Solution

Case 1:
If total number of stones is 4k, then who ever starts the game will lose. Let's say player A starts the game, and at each round he remove n stones, where n is [1, 3]. Player B who starts later can always remove 4 - n stones, where 4 - n is [1, 3]. So after each round, total number of stones is reduced to 4(k - 1). Therefore player B will close the game and player A will lose.

Case 2:
If total number of stones is 4k + p, where p is [1, 3], then who ever starts the game can win. At the first round, player A can remove p stones. The problem is now converted to case 1 and therefore player B will lose the game.

```java
public class Solution {
    public boolean canWinNim(int n) {
		return n % 4 != 0;
    }
}
```
