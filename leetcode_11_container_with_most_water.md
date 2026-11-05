# 🎯 LeetCode 11: Container With Most Water

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the $i^{th}$ line are `(i, 0)` and `(i, height[i])`.

Find two lines that, together with the x-axis, form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

#### Examples

- **Example 1**:
  - **Input**: `height = [1,8,6,2,5,4,8,3,7]`
  - **Output**: `49`
  - **Explanation**: The vertical lines are represented by array `[1,8,6,2,5,4,8,3,7]`. In this case, the max area of water the container can contain is $49$ (between index 1 and index 8, with height $\min(8, 7) = 7$ and width $8 - 1 = 7$).

- **Example 2**:
  - **Input**: `height = [1,1]`
  - **Output**: `1`

#### Constraints

- $n == \text{height.length}$
- $2 \le n \le 10^5$
- $0 \le \text{height}[i] \le 10^4$

---

### 💡 Intuition & Strategy

1. **Two-Pointer Setup**:
   - Initialize two pointers: `l` (left) at the start index `0` and `r` (right) at the end index `height.length - 1`.
   - This maximizes the initial width of the container.

2. **Area Calculation & Optimization**:
   - The area of water stored between two lines is determined by the formula:
     $$\text{Area} = (\text{right} - \text{left}) \times \min(\text{height}[l], \text{height}[r])$$
   - Calculate the current capacity and update the global `max_capacity` if the current area is greater.

3. **Pointer Movement Logic**:
   - Since the area is bounded by the shorter line, moving the pointer at the taller line inward cannot possibly increase the area (as the width decreases and the limiting height can at best stay the same or drop).
   - Therefore, increment `l` (`l++`) if `heights[l] < heights[r]`, otherwise decrement `r` (`r--`), to search for a taller bounding line.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Each element is visited at most once by either the left or right pointer, leading to a linear scan.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Only a few primitive pointer and integer variables are used.

---

### 💻 Java Solution

```java
/**
 * Problem: Container With Most Water (LeetCode 11)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int maxArea(int[] heights) {
        int l = 0;
        int r = heights.length - 1;
        int max_capacity = 0;
        int curr_cap = 0;

        while (l < r) {
            int width = r - l;
            int height = Math.min(heights[l], heights[r]);
            curr_cap = width * height;

            max_capacity = Math.max(max_capacity, curr_cap);

            if (heights[l] < heights[r]) {
                l++;
            } else {
                r--;
            }
        }
        return max_capacity;
    }
}
```