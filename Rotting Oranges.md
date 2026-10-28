# 🎯 LeetCode 994: Rotting Oranges

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given an `m x n` grid where each cell can have one of three values:
- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return `-1`.

#### Examples

- **Example 1**:
  - **Input**: `grid = [[2,1,1],[1,1,0],[0,1,1]]`
  - **Output**: `4`

- **Example 2**:
  - **Input**: `grid = [[2,1,1],[0,1,1],[1,0,1]]`
  - **Output**: `-1`
  - **Explanation**: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

- **Example 3**:
  - **Input**: `grid = [[0,2]]`
  - **Output**: `0`
  - **Explanation**: Since there are already no fresh oranges at minute 0, the answer is just 0.

#### Constraints

- $m == \text{grid.length}$
- $n == \text{grid}[i]\text{.length}$
- $1 \le m, n \le 10$
- $\text{grid}[i][j]$ is $0$, $1$, or $2$.

---

### 💡 Intuition & Strategy

1. **Multi-Source Breadth-First Search (BFS)**:
   - Since multiple oranges can rot simultaneously and the "infection" spreads one step at a time, this is a classic shortest-path/level-order traversal problem.
   - We use BFS to simulate the rotting process minute by minute across all initially rotten oranges at the same time.

2. **Initialization phase**:
   - Traverse the entire grid to count the total number of `freshOranges`.
   - Simultaneously, add the coordinates of all initially `rottenOranges` (value `2`) to a Queue. These act as our BFS starting points (Level 0).

3. **Level-by-Level Expansion**:
   - Process the queue level by level. Each level represents one `minute`.
   - For every rotten orange polled from the queue, check its 4-directional neighbors (up, down, left, right).
   - If a neighbor is a fresh orange (value `1`), turn it rotten (value `2`), decrement the `freshOranges` count, and enqueue its coordinates for the next minute's processing.

4. **Termination & Verification**:
   - The BFS stops when the queue is empty or no fresh oranges are left.
   - Finally, check the `freshOranges` count. If it is `0`, return the elapsed `minutes`. If it is greater than `0` (meaning some oranges were unreachable), return `-1`.

5. **Complexity**:
   - **Time Complexity**: $O(M \times N)$
     - We traverse the grid once to initialize the queue, and at most once during the BFS. Each cell is visited a constant number of times.
   - **Space Complexity**: $O(M \times N)$
     - In the worst case (e.g., all oranges are rotten initially), the queue will hold all $M \times N$ cells.

---

### 💻 Java Solution

```java
/**
 * Problem: Rotting Oranges (LeetCode 994)
 * Language: Java 17
 * Time Complexity: O(M * N)
 * Space Complexity: O(M * N) worst case
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public int orangesRotting(int[][] grid) {
        if (grid == null || grid.length == 0) return 0;
        
        int m = grid.length;
        int n = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int freshOranges = 0;
        
        // Step 1: Initialize the queue with all initially rotten oranges 
        // and count the total number of fresh oranges.
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) {
                    queue.offer(new int[]{i, j});
                } else if (grid[i][j] == 1) {
                    freshOranges++;
                }
            }
        }
        
        // If there are no fresh oranges to begin with, it takes 0 minutes.
        if (freshOranges == 0) return 0;
        
        int minutes = 0;
        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        
        // Step 2: Perform BFS level by level
        while (!queue.isEmpty() && freshOranges > 0) {
            int size = queue.size();
            
            for (int i = 0; i < size; i++) {
                int[] current = queue.poll();
                
                // Check all 4 adjacent directions
                for (int[] dir : directions) {
                    int x = current[0] + dir[0];
                    int y = current[1] + dir[1];
                    
                    // If the adjacent cell is within bounds and has a fresh orange
                    if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                        grid[x][y] = 2; // It becomes rotten
                        freshOranges--;
                        queue.offer(new int[]{x, y});
                    }
                }
            }
            minutes++;
        }
        
        // Step 3: If there are still fresh oranges left, return -1. Otherwise, return minutes.
        return freshOranges == 0 ? minutes : -1;
    }
}
```
