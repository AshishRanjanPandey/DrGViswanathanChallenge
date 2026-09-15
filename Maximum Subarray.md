# 🎯 LeetCode 53: Maximum Subarray

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an integer array `nums`, find the subarray with the largest sum, and return its sum.

#### Examples

- **Example 1**:
  - **Input**: `nums = [-2,1,-3,4,-1,2,1,-5,4]`
  - **Output**: `6`
  - **Explanation**: The subarray `[4,-1,2,1]` has the largest sum `6`.

- **Example 2**:
  - **Input**: `nums = [1]`
  - **Output**: `1`
  - **Explanation**: The subarray `[1]` has the largest sum `1`.

- **Example 3**:
  - **Input**: `nums = [5,4,-1,7,8]`
  - **Output**: `23`
  - **Explanation**: The subarray `[5,4,-1,7,8]` has the largest sum `23`.

#### Constraints

- $1 \le \text{nums.length} \le 10^5$
- $-10^4 \le \text{nums}[i] \le 10^4$

---

### 💡 Intuition & Strategy

1. **Kadane's Algorithm (Dynamic Programming / Greedy)**:
   - A brute-force approach checking all possible contiguous subarrays takes $O(N^2)$ time, which will result in Time Limit Exceeded (TLE) for $N = 10^5$.
   - At each index $i$, we face a binary choice for extending the subarray:
     - Add `nums[i]` to the accumulated `currentSum`.
     - Discard the previous subarray sum entirely and start a new subarray at `nums[i]`.
   - Mathematically, the recurrence relation is:
     $$\text{currentSum}_i = \max(\text{nums}[i], \text{currentSum}_{i-1} + \text{nums}[i])$$

2. **State Maintenance**:
   - Initialize `currentSum` to `nums[0]`.
   - Initialize `maxSum` to `nums[0]`.
   - Initializing with `nums[0]` avoids sentinel issues (e.g., arrays where all elements are negative).

3. **Dynamic Evaluation**:
   - Iterate from index $1$ through $N - 1$:
     - Update `currentSum = Math.max(nums[i], currentSum + nums[i])`.
     - Update `maxSum = Math.max(maxSum, currentSum)`.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Single linear pass through the array.
   - **Space Complexity**: $O(1)$
     - Requires only two scalar integer tracking variables.

---

### 💻 Java Solution

```java
/**
 * Problem: Maximum Subarray (LeetCode 53)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {

    public int maxSubArray(int[] nums) {
        int currentSum = nums[0];
        int maxSum = nums[0];

        for (int i = 1; i < nums.length; i++) {
            currentSum = Math.max(nums[i], currentSum + nums[i]);
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }
}
