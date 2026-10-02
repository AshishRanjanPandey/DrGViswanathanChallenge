# 🎯 LeetCode 733: Flood Fill (BFS Approach)

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given an $m \times n$ grid of integers `image`, where `image[i][j]` represents the pixel value of the image. You are also given three integers `sr`, `sc`, and `color`. Your task is to perform a flood fill on the image starting from the pixel `image[sr][sc]`.

To perform a flood fill:
1. Begin with the starting pixel and change its color to `color`.
2. Perform the same process for each pixel that is directly adjacent (pixels that share a side with the original pixel, either horizontally or vertically) and shares the same color as the starting pixel.
3. Keep repeating this process by checking neighboring pixels of the updated pixels and modifying their color if it matches the original color of the starting pixel.
4. The process stops when there are no more adjacent pixels of the original color to update.

Return the modified image after performing the flood fill.

#### Examples

- **Example 1**:
  - **Input**: `image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2`
  - **Output**: `[[2,2,2],[2,2,0],[2,0,1]]`
  - **Explanation**: From the center of the image with position `(sr, sc) = (1, 1)`, all pixels connected by a path of the same color are colored with the new color. Note that the bottom corner is not colored `2` because it is not horizontally or vertically connected to the starting pixel.

- **Example 2**:
  - **Input**: `image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0`
  - **Output**: `[[0,0,0],[0,0,0]]`
  - **Explanation**: The starting pixel is already colored with `0`, which is the same as the target color. Therefore, no changes are made to the image.

#### Constraints

- $m == \text{image.length}$
- $n == \text{image[i].length}$
- $1 \le m, n \le 50$
- $0 \le \text{image[i][j]}, \text{color} < 2^{16}$
- $0 \le sr < m$
- $0 \le sc < n$

---

### 💡 Intuition & Strategy

1. **Modeling via Breadth-First Search (BFS)**:
   - Instead of recursion (which uses the call stack), we can use an iterative queue-based approach (BFS) to explore layer-by-layer outwards from the starting pixel.

2. **Avoiding Infinite Loops & Tracking Visited Nodes**:
   - If the target `color` equals the starting pixel's `originalColor`, we can immediately return the image.
   - We repaint each valid neighbor as we push it onto the queue, which naturally serves as our "visited" state, preventing redundant processing.

3. **Direction Vectors**:
   - We use direction arrays `dr` and `dc` to systematically check all 4 orthogonal neighbors (Up, Down, Left, Right).

4. **Complexity**:
   - **Time Complexity**: $O(N)$, where $N$ is the total number of pixels ($m \times n$). Every pixel is added to and removed from the queue at most once.
   - **Space Complexity**: $O(N)$ in the worst-case scenario when the queue holds all pixels (e.g., if the entire grid is filled with the same color).

---

### 💻 Java Solution (BFS)

```java
/**
 * Problem: Flood Fill (LeetCode 733) - BFS Iterative Approach
 * Language: Java 17
 * Time Complexity: O(N) where N is the total number of pixels
 * Space Complexity: O(N) auxiliary queue space
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        int originalColor = image[sr][sc];
        
        // If the original color is already the target color, no change is needed
        if (originalColor == color) {
            return image;
        }
        
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{sr, sc});
        image[sr][sc] = color; // Mark as visited by changing color immediately
        
        // Direction arrays for 4-way connectivity (Up, Down, Left, Right)
        int[] dr = {-1, 1, 0, 0};
        int[] dc = {0, 0, -1, 1};
        
        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            int r = curr[0];
            int c = curr[1];
            
            for (int i = 0; i < 4; i++) {
                int nr = r + dr[i];
                int nc = c + dc[i];
                
                // Check boundaries and whether the neighbor matches the original color
                if (nr >= 0 && nr < image.length && nc >= 0 && nc < image[0].length && image[nr][nc] == originalColor) {
                    image[nr][nc] = color; // Repaint before pushing to prevent re-adding
                    queue.offer(new int[]{nr, nc});
                }
            }
        }
        
        return image;
    }
}
