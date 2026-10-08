# 🎯 LeetCode 198: House Robber

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. The only constraint stopping you from robbing each of them is that adjacent houses have security systems connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

#### Examples

- **Example 1**:
  - **Input**: `nums = [1,2,3,1]`
  - **Output**: `4`
  - **Explanation**: Rob house 1 (money = 1) and then rob house 3 (money = 3). Total amount you can rob = $1 + 3 = 4$.

- **Example 2**:
  - **Input**: `nums = [2,7,9,3,1]`
  - **Output**: `12`
  - **Explanation**: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1). Total amount you can rob = $2 + 9 + 1 = 12$.

#### Constraints

- $1 \le \text{nums.length} \le 100$
- $0 \le \text{nums}[i] \le 400$

---

### 💡 Intuition & Strategy

1. **Dynamic Programming Choices**:
   - At any given house $i$, you are faced with a choice:
     1. **Skip the current house**: Your maximum loot remains whatever you accumulated up to the previous house ($\text{prev1}$).
     2. **Rob the current house**: You add the current house's loot ($\text{num}$) to the maximum loot you had up to two houses ago ($\text{prev2}$).
   - This leads to the recurrence relation: $\text{current} = \max(\text{prev1}, \text{prev2} + \text{num})$.

2. **Space Optimization ($O(1)$ Space)**:
   - Notice that to calculate the maximum loot for house $i$, we only ever need the results from the immediately preceding house ($\text{prev1}$) and the house before that ($\text{prev2}$).
   - Instead of maintaining an entire DP array of size $N$, we can use two scalar variables to roll forward through the array, reducing auxiliary space complexity to constant memory.

3. **Complexity**:
   - **Time Complexity**: $O(N)$
     - We iterate through the array of length $N$ exactly once.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Only a constant number of variables (`prev1`, `prev2`, `current`) are used regardless of input size.

---

### 💻 Java Solution

```java
/**
 * Problem: House Robber (LeetCode 198)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        // prev2 represents the max loot up to i-2, prev1 represents max loot up to i-1
        int prev2 = 0;
        int prev1 = 0;
        
        for (int num : nums) {
            // Choose the maximum between skipping the current house or robbing it
            int current = Math.max(prev1, prev2 + num);
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
}
