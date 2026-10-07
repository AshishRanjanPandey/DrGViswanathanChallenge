# 🎯 LeetCode 463: Island Perimeter

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given a row x col grid representing a map where `grid[i][j] = 1` represents land and `grid[i][j] = 0` represents water. Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells). 

The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

#### Examples

- **Example 1**:
  - **Input**: `grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]`
  - **Output**: `16`
  - **Explanation**: The perimeter is the 16 yellow stripes surrounding the land cells in the grid.

- **Example 2**:
  - **Input**: `grid = [[1]]`
  - **Output**: `4`

- **Example 3**:
  - **Input**: `grid = [[1,0]]`
  - **Output**: `4`

#### Constraints

- `row == grid.length`
- `col == grid[i].length`
- `1 <= row, col <= 100`
- `grid[i][j]` is `0` or `1`.
- There is exactly one island in `grid`.

---

### 💡 Intuition & Strategy

1. **Cell-by-Cell Grid Traversal**:
   - Iterate through every cell in the 2D grid using a nested loop.
   - Whenever we encounter a land cell (`grid[i][j] == 1`), it represents a square block that potentially contributes to the perimeter.

2. **Checking Four Cardinal Directions**:
   - For every land cell, check its four immediate neighbors: Up, Down, Left, and Right.
   - An edge contributes $1$ to the perimeter if the neighbor in that direction is **out of bounds** or is **water (`0`)**.

3. **Complexity**:
   - **Time Complexity**: $O(\text{row} \times \text{col})$
     - We visit every cell in the grid exactly once, performing constant time $O(1)$ checks for its four neighbors.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Only a few primitive variables (`rows`, `cols`, `perimeter`) are used, requiring no extra data structures.

---

### 💻 Java Solution

```java
/**
 * Problem: Island Perimeter (LeetCode 463)
 * Language: Java 17
 * Time Complexity: O(row * col)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int islandPerimeter(int[][] grid) {
        int rows = grid.length;
        int cols = grid[0].length;
        int perimeter = 0;
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 1) {
                    // Check all 4 directions (up, down, left, right)
                    
                    // Up neighbor
                    if (i == 0 || grid[i - 1][j] == 0) {
                        perimeter++;
                    }
                    // Down neighbor
                    if (i == rows - 1 || grid[i + 1][j] == 0) {
                        perimeter++;
                    }
                    // Left neighbor
                    if (j == 0 || grid[i][j - 1] == 0) {
                        perimeter++;
                    }
                    // Right neighbor
                    if (j == cols - 1 || grid[i][j + 1] == 0) {
                        perimeter++;
                    }
                }
            }
        }
        
        return perimeter;
    }
}
