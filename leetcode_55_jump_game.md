# 🎯 LeetCode 55: Jump Game

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

#### Examples

- **Example 1**:
  - **Input**: `nums = [2,3,1,1,4]`
  - **Output**: `true`
  - **Explanation**: Jump $1$ step from index $0$ to $1$, then $3$ steps to the last index.

- **Example 2**:
  - **Input**: `nums = [3,2,1,0,4]`
  - **Output**: `false`
  - **Explanation**: You will always arrive at index $3$ no matter what. Its maximum jump length is $0$, which makes it impossible to reach the last index.

#### Constraints

- $1 \le \text{nums.length} \le 10^4$
- $0 \le \text{nums}[i] \le 10^5$

---

### 💡 Intuition & Strategy

1. **Greedy Approach**:
   - Instead of checking every possible jump path (which would be $O(2^N)$ time complexity and result in a Time Limit Exceeded error), we can use a greedy strategy to simply track the furthest index we can possibly reach at any given point.

2. **Tracking Maximum Reach**:
   - Maintain a variable `maxReach` initialized to $0$.
   - Iterate through the array. For each index `i`, check if `i > maxReach`. If it is, this means there is an unavoidable gap of zeroes that we couldn't jump over. We are stuck, so we return `false`.

3. **Updating the Boundary**:
   - If we can reach the current index, we calculate the furthest index reachable from this step, which is `i + nums[i]`.
   - Update `maxReach` to be the maximum of its current value and this new reach.
   - Early exit: If `maxReach` ever becomes greater than or equal to the last index (`nums.length - 1`), we know it's possible to reach the end, and we immediately return `true`.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - We only make a single pass through the array.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - We strictly use one integer variable (`maxReach`) to keep track of our state, requiring no extra space that scales with the input size.

---

### 💻 Java Solution

```java
/**
 * Problem: Jump Game (LeetCode 55)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;
        
        for (int i = 0; i < nums.length; i++) {
            // If the current index is beyond our maximum reachable index, we are stuck
            if (i > maxReach) {
                return false;
            }
            
            // Update the maximum reachable index
            maxReach = Math.max(maxReach, i + nums[i]);
            
            // Early exit if we can already reach or pass the last index
            if (maxReach >= nums.length - 1) {
                return true;
            }
        }
        
        return true;
    }
}
```