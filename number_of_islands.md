# 🎯 LeetCode 200: Number of Islands

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an $m \times n$ 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

#### Examples

- **Example 1**:
  - **Input**: `grid = [["1","1","1","1","0"],["1","1","0","1","0"],["1","1","0","0","0"],["0","0","0","0","0"]]`
  - **Output**: `1`
  - **Explanation**: The entire landmass forms a single connected island.

- **Example 2**:
  - **Input**: `grid = [["1","1","0","0","0"],["1","1","0","0","0"],["0","0","1","0","0"],["0","0","0","1","1"]]`
  - **Output**: `3`
  - **Explanation**: There are three distinct islands separated by water.

#### Constraints

- $m == \text{grid.length}$
- $n == \text{grid}[i].\text{length}$
- $1 \le m, n \le 300$
- `grid[i][j]` is `'0'` or `'1'`.

---

### 💡 Intuition & Strategy

1. **Grid Traversal via Nested Loops**:
   - We scan through every cell in the 2D grid row by row, column by column.
   - When we encounter a cell containing `'1'` (land), it signifies the discovery of a new, unvisited island. We increment our `islandCount`.

2. **Sinking the Island using Depth-First Search (DFS)**:
   - To prevent counting the same island multiple times, every time we find a land cell, we launch a DFS helper function to explore and "sink" all connected land cells.
   - "Sinking" involves mutating the grid value from `'1'` to `'0'`, effectively marking those cells as visited without needing an extra visited boolean array.

3. **Recursive Exploration & Boundary Checks**:
   - The DFS function checks if the current cell coordinates $(r, c)$ are out of bounds or if the cell is water (`'0'`). If so, it returns.
   - Otherwise, it recursively explores all 4 adjacent directions: Up, Down, Left, and Right.

4. **Complexity**:
   - **Time Complexity**: $\mathcal{O}(M \times N)$
     - Every cell in the $M \times N$ grid is visited a constant number of times (once by the outer loops, and at most once during the DFS traversal).
   - **Space Complexity**: $\mathcal{O}(M \times N)$ auxiliary memory
     - In the worst-case scenario (e.g., the entire grid is filled with land), the recursive call stack can grow up to $M \times N$ deep.

---

### 💻 Java Solution

```java
/**
 * Problem: Number of Islands (LeetCode 200)
 * Language: Java 17
 * Time Complexity: O(M * N)
 * Space Complexity: O(M * N) due to recursive call stack
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int rows = grid.length;
        int cols = grid[0].length;
        int islandCount = 0;

        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == '1') {
                    islandCount++;
                    dfs(grid, r, c); // Mark all connected land cells as visited
                }
            }
        }

        return islandCount;
    }

    private void dfs(char[][] grid, int r, int c) {
        int rows = grid.length;
        int cols = grid[0].length;

        // Base case: check bounds and if the current cell is water ('0')
        if (r < 0 || c < 0 || r >= rows || c >= cols || grid[r][c] == '0') {
            return;
        }

        // Mark the current cell as visited by turning it into '0'
        grid[r][c] = '0';

        // Recursively visit all 4 adjacent directions
        dfs(grid, r - 1, c); // Up
        dfs(grid, r + 1, c); // Down
        dfs(grid, r, c - 1); // Left
        dfs(grid, r, c + 1); // Right
    }
}