# 🎯 LeetCode 279: Perfect Squares

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an integer `n`, return the least number of perfect square numbers that sum to `n`.

A perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, 1, 4, 9, and 16 are perfect squares while 3 and 11 are not.

#### Examples

- **Example 1**:
  - **Input**: `n = 12`
  - **Output**: `3`
  - **Explanation**: $12 = 4 + 4 + 4$.

- **Example 2**:
  - **Input**: `n = 13`
  - **Output**: `2`
  - **Explanation**: $13 = 4 + 9$.

#### Constraints

- $1 \le n \le 10^4$

---

### 💡 Intuition & Strategy

1. **Dynamic Programming Approach**:
   - The problem asks for the minimum number of elements (perfect squares) to achieve a target sum (`n`), which exhibits optimal substructure. This makes it a perfect candidate for Dynamic Programming (similar to the Coin Change problem).

2. **Defining the State**:
   - Create an array `dp` of size `n + 1`, where `dp[i]` represents the least number of perfect squares that sum to the value `i`.
   - Initialize all elements to a maximum value (like `Integer.MAX_VALUE`) to represent uncalculated states, except for `dp[0] = 0` (0 requires zero perfect squares).

3. **Building the Solution (Bottom-Up)**:
   - Iterate through each number `i` from $1$ to `n`.
   - For each `i`, test every possible perfect square $j^2$ that is less than or equal to `i` (i.e., $j \times j \le i$).
   - Update the state using the transition: `dp[i] = Math.min(dp[i], dp[i - j * j] + 1)`. This means we are taking one perfect square ($j^2$) and adding it to the optimal solution for the remainder (`i - j * j`).

4. **Complexity**:
   - **Time Complexity**: $O(N \sqrt{N})$
     - The outer loop runs $N$ times. For each iteration `i`, the inner loop runs $\sqrt{i}$ times. Summing this up results in bounded $O(N \sqrt{N})$ operations.
   - **Space Complexity**: $O(N)$
     - We use a 1D DP array of size $N + 1$ to store the intermediate answers.

---

### 💻 Java Solution

```java
/**
 * Problem: Perfect Squares (LeetCode 279)
 * Language: Java 17
 * Time Complexity: O(N * sqrt(N))
 * Space Complexity: O(N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.Arrays;

class Solution {
    public int numSquares(int n) {
        // dp[i] represents the least number of perfect squares that sum to i
        int[] dp = new int[n + 1];
        
        // Initialize the array with a maximum value
        Arrays.fill(dp, Integer.MAX_VALUE);
        
        // Base case: 0 requires 0 perfect squares
        dp[0] = 0;
        
        // Build the dp array from 1 to n
        for (int i = 1; i <= n; i++) {
            // For each i, check all perfect squares (j*j) that are <= i
            for (int j = 1; j * j <= i; j++) {
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
            }
        }
        
        return dp[n];
    }
}
```
