# 🎯 LeetCode 84: Largest Rectangle in Histogram

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return the area of the largest rectangle in the histogram.

#### Examples

- **Example 1**:
  - **Input**: `heights = [2,1,5,6,2,3]`
  - **Output**: `10`
  - **Explanation**: The heights `[5,6]` form a rectangle of width `2` and height `5`, yielding $\text{area} = 5 \times 2 = 10$.

- **Example 2**:
  - **Input**: `heights = [2,4]`
  - **Output**: `4`
  - **Explanation**: The bar of height `4` with width `1` yields $\text{area} = 4$, or height `2` with width `2` yields $\text{area} = 2 \times 2 = 4$.

#### Constraints

- $1 \le \text{heights.length} \le 10^5$
- $0 \le \text{heights}[i] \le 10^4$

---

### 💡 Intuition & Strategy

1. **Monotonic Stack Approach**:
  - To compute the largest rectangle formed by a bar at index `hIndex` as the limiting (shortest) height, we must identify the nearest bar strictly smaller than it to its left and to its right.
  - A **monotonic increasing stack** storing indices allows us to track these boundary elements efficiently in a single pass.

2. **Popping & Rectangle Area Calculation**:
  - Iterate index `i` from `0` to `n` (where `i = n` uses a sentinel virtual height of `0`).
  - While the stack is non-empty and the current height `heights[i]` is smaller than `heights[stack.peek()]`:
    - Pop the top index `hIndex = stack.pop()`.
    - **Height**: `heights[hIndex]`
    - **Width**: If the stack becomes empty, the rectangle extends all the way back to index `0` (width = `i`). Otherwise, width = `i - stack.peek() - 1`.
    - **Area**: `height * width`
    - Update `maxArea` if this area exceeds the previous maximum.

3. **Sentinel Value Optimization**:
  - Using a sentinel height of `0` at index `n` automatically flushes any remaining bars left in the stack without requiring extra post-processing code outside the loop.

4. **Complexity**:
  - **Time Complexity**: $O(N)$
    - Each element index is pushed onto and popped from the stack at most once, yielding $O(N)$ linear time.
  - **Space Complexity**: $O(N)$ auxiliary memory
    - In the worst case (strictly increasing heights), the stack stores up to $N$ indices.

---

### 💻 Java Solution

```java
/**
- Problem: Largest Rectangle in Histogram (LeetCode 84)
- Language: Java 17
- Time Complexity: O(N)
- Space Complexity: O(N) auxiliary
- Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int largestRectangleArea(int[] heights) {
        Deque<Integer> stack = new ArrayDeque<>();
        int maxArea = 0;
        int n = heights.length;

        for (int i = 0; i <= n; i++) {
            // Virtual sentinel height 0 at index n to flush remaining stack elements
            int currentHeight = (i == n) ? 0 : heights[i];

            while (!stack.isEmpty() && currentHeight < heights[stack.peek()]) {
                int height = heights[stack.pop()];
                int width = stack.isEmpty() ? i : i - stack.peek() - 1;
                maxArea = Math.max(maxArea, height * width);
            }

            stack.push(i);
        }

        return maxArea;
    }
}
```