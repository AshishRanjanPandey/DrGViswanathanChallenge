# 🎯 LeetCode 417: Pacific Atlantic Water Flow

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

There is an `m x n` rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the height above sea level of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is **less than or equal to** the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates `result` where `result[i] = [ri, ci]` denotes that rain water can flow from cell `(ri, ci)` to **both** the Pacific and Atlantic oceans.

#### Examples

- **Example 1**:
  - **Input**: `heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]`
  - **Output**: `[[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]`
  - **Explanation**: 
    - `[0,4]`: `[0,4]` -> Pacific Ocean AND `[0,4]` -> Atlantic Ocean
    - `[1,3]`: `[1,3]` -> `[0,3]` -> Pacific Ocean AND `[1,3]` -> `[1,4]` -> Atlantic Ocean
    - `[1,4]`: `[1,4]` -> `[1,3]` -> `[0,3]` -> Pacific Ocean AND `[1,4]` -> Atlantic Ocean
    - `[2,2]`: `[2,2]` -> `[1,2]` -> `[0,2]` -> Pacific Ocean AND `[2,2]` -> `[2,3]` -> `[2,4]` -> Atlantic Ocean
    - `[3,0]`: `[3,0]` -> Pacific Ocean AND `[3,0]` -> `[4,0]` -> Atlantic Ocean
    - `[3,1]`: `[3,1]` -> `[3,0]` -> Pacific Ocean AND `[3,1]` -> `[4,1]` -> Atlantic Ocean
    - `[4,0]`: `[4,0]` -> Pacific Ocean AND `[4,0]` -> Atlantic Ocean

- **Example 2**:
  - **Input**: `heights = [[1]]`
  - **Output**: `[[0,0]]`
  - **Explanation**: The water can flow from the only cell to the Pacific and Atlantic oceans.

#### Constraints

- `m == heights.length`
- `n == heights[r].length`
- $1 \le m, n \le 200$
- $0 \le heights[r][c] \le 10^5$

---

### 💡 Intuition & Strategy

1. **Reverse the Problem (Ocean to Island)**:
   - Instead of simulating water flowing "down" from every single cell (which recalculates paths redundantly), we can reverse the flow.
   - We start from the ocean edges and flow "up" to the cells. A cell can reach an ocean if the ocean can reach the cell by moving to neighbors with a height **greater than or equal to** the current cell.

2. **Depth-First Search (DFS) or Breadth-First Search (BFS)**:
   - Run a DFS starting from all cells on the top and left edges, marking all reachable cells as connected to the **Pacific Ocean**.
   - Run a separate DFS starting from all cells on the bottom and right edges, marking all reachable cells as connected to the **Atlantic Ocean**.

3. **Finding the Intersection**:
   - Maintain two boolean matrices: `pacific` and `atlantic`.
   - After completing the DFS traversals from the edges, iterate through the entire grid.
   - If a cell `(i, j)` is true in both the `pacific` and `atlantic` matrices, it means water can flow to both oceans. Add its coordinates to the result list.

4. **Complexity**:
   - **Time Complexity**: $O(m \times n)$
     - We visit every cell in the grid at most a constant number of times during the DFS traversals.
   - **Space Complexity**: $O(m \times n)$
     - We use two $m \times n$ boolean matrices to track reachable cells.
     - The implicit call stack for DFS can also go up to $O(m \times n)$ deep in the worst case (e.g., a grid where heights strictly increase in a spiral).

---

### 💻 Java Solution

```java
/**
 * Problem: Pacific Atlantic Water Flow (LeetCode 417)
 * Language: Java 17
 * Time Complexity: O(m * n)
 * Space Complexity: O(m * n)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    private static final int[][] DIRECTIONS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        List<List<Integer>> result = new ArrayList<>();
        if (heights == null || heights.length == 0 || heights[0].length == 0) {
            return result;
        }

        int m = heights.length;
        int n = heights[0].length;

        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];

        // DFS from the left edge (Pacific) and right edge (Atlantic)
        for (int i = 0; i < m; i++) {
            dfs(heights, pacific, i, 0);
            dfs(heights, atlantic, i, n - 1);
        }

        // DFS from the top edge (Pacific) and bottom edge (Atlantic)
        for (int j = 0; j < n; j++) {
            dfs(heights, pacific, 0, j);
            dfs(heights, atlantic, m - 1, j);
        }

        // Find cells that can reach both oceans
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] && atlantic[i][j]) {
                    result.add(Arrays.asList(i, j));
                }
            }
        }

        return result;
    }

    private void dfs(int[][] heights, boolean[][] visited, int r, int c) {
        visited[r][c] = true;
        
        for (int[] dir : DIRECTIONS) {
            int newRow = r + dir[0];
            int newCol = c + dir[1];
            
            // In reverse flow, water can move if the neighbor is HIGHER OR EQUAL
            if (newRow >= 0 && newRow < heights.length && 
                newCol >= 0 && newCol < heights[0].length && 
                !visited[newRow][newCol] && 
                heights[newRow][newCol] >= heights[r][c]) {
                
                dfs(heights, visited, newRow, newCol);
            }
        }
    }
}
