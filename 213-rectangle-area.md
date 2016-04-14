# [223. Rectangle Area](https://leetcode.com/problems/rectangle-area/)

Find the total area covered by two rectilinear rectangles in a 2D plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

Rectangle Area

Assume that the total area is never beyond the maximum possible value of int.

## Solution 1

```java
public class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
		int bottomLeftX = Math.max(A, E);
		int bottomLeftY = Math.max(B, F);
		int topRightX = Math.min(C, G);
		int topRightY = Math.min(D, H);
		long intersection = bottomLeftX < topRightX && bottomLeftY < topRightY ?
			area(bottomLeftX, bottomLeftY, topRightX, topRightY) :
			0;
		return (int)(area(A, B, C, D) + area(E, F, G, H) - intersection);
    }

	private long area(int x1, int y1, int x2, int y2) {
		return (x2 - x1) * (y2 - y1);
	}
}
```

## Solution 2

```java
public class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
		int l = Math.min(Math.max(A, E), C);
		int r = Math.max(Math.min(C, G), A);
		int b = Math.min(Math.max(B, F), D);
		int t = Math.max(Math.min(D, H), B);
		return (C - A) * (D - B) + (G - E) * (H - F) - (r - l) * (t - b);
	}
}
```
