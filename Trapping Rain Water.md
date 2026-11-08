# 🎯 LeetCode 42: Trapping Rain Water

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given $n$ non-negative integers representing an elevation map where the width of each bar is $1$, compute how much water it can trap after raining.

#### Examples

- **Example 1**:
  - **Input**: `height = [0,1,0,2,1,0,1,3,2,1,2,1]`
  - **Output**: `6`
  - **Explanation**: The elevation map is represented by array `[0,1,0,2,1,0,1,3,2,1,2,1]`. In this case, $6$ units of rain water are being trapped.

- **Example 2**:
  - **Input**: `height = [4,2,0,3,2,5]`
  - **Output**: `9`

#### Constraints

- $n == \text{height.length}$
- $1 \le n \le 2 \times 10^4$
- $0 \le \text{height}[i] \le 10^5$

---

### 💡 Intuition & Strategy

1. **The Core Concept**:
   - The amount of water trapped above any given bar depends on the maximum height of bars to its left and the maximum height of bars to its right. 
   - Specifically, the water at index `i` is bounded by $\min(\text{leftMax}, \text{rightMax}) - \text{height}[i]$.

2. **Two-Pointer Setup**:
   - Place one pointer at the start (`left = 0`) and one at the end (`right = height.length - 1`) of the elevation map.
   - Maintain two variables, `leftMax` and `rightMax`, to record the highest bars seen so far from both sides.

3. **Comparing and Computing**:
   - Compare `height[left]` and `height[right]`:
     - If `height[left] < height[right]`: The bottleneck for trapped water at the `left` pointer is determined solely by `leftMax` (since we know a taller boundary exists on the right). We process the left side, add any trapped water, update `leftMax`, and advance `left`.
     - If `height[left] \ge height[right]`: The bottleneck is determined by `rightMax` for the same reason. We process the right side, add trapped water, update `rightMax`, and retreat `right`.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - We iterate through the `height` array exactly once using the two pointers.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - We only use a few integer variables (`left`, `right`, `leftMax`, `rightMax`, `trappedWater`) regardless of the input size.

---

### 💻 Java Solution

```java
/**
 * Problem: Trapping Rain Water (LeetCode 42)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int trap(int[] height) {
        if (height == null || height.length == 0) {
            return 0;
        }
        
        int left = 0;
        int right = height.length - 1;
        
        int leftMax = 0;
        int rightMax = 0;
        
        int trappedWater = 0;
        
        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= leftMax) {
                    // Update max height on the left
                    leftMax = height[left];
                } else {
                    // Water trapped is the difference between leftMax and current height
                    trappedWater += leftMax - height[left];
                }
                left++;
            } else {
                if (height[right] >= rightMax) {
                    // Update max height on the right
                    rightMax = height[right];
                } else {
                    // Water trapped is the difference between rightMax and current height
                    trappedWater += rightMax - height[right];
                }
                right--;
            }
        }
        
        return trappedWater;
    }
}
```
