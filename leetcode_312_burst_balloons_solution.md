# 🎯 LeetCode 312: Burst Balloons

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given $n$ balloons, indexed from $0$ to $n - 1$. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the $i^{\text{th}}$ balloon, you will get $\text{nums}[i - 1] \times \text{nums}[i] \times \text{nums}[i + 1]$ coins. If $i - 1$ or $i + 1$ goes out of bounds of the array, then treat it as if there is a balloon with a $1$ painted on it.

Return the maximum coins you can collect by bursting the balloons wisely.

#### Examples

- **Example 1**:
  - **Input**: `nums = [3,1,5,8]`
  - **Output**: `167`
  - **Explanation**: 
    - `nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []`
    - $\text{coins} = (3 \times 1 \times 5) + (3 \times 5 \times 8) + (1 \times 3 \times 8) + (1 \times 8 \times 1) = 15 + 120 + 24 + 8 = 167$

- **Example 2**:
  - **Input**: `nums = [1,5]`
  - **Output**: `10`
  - **Explanation**: 
    - `nums = [1,5] --> [5] --> []`
    - $\text{coins} = (1 \times 1 \times 5) + (1 \times 5 \times 1) = 5 + 5 = 10$

#### Constraints

- $n == \text{nums.length}$
- $1 \le n \le 300$
- $0 \le \text{nums}[i] \le 100$

---

### 💡 Intuition & Strategy

1. **Reverse Perspective (Last Balloon Burst)**:
  - Bursting balloons dynamically changes adjacent elements, introducing dependent subproblems if approached top-down (choosing which balloon to burst *first*).
  - Instead, work backwards: ask **"Which balloon $k$ inside the open interval $(left, right)$ is burst LAST?"**
  - If balloon $k$ is the last to burst within $(left, right)$, all other balloons in $(left, k)$ and $(k, right)$ are already gone. Therefore, the neighbors of $k$ at the moment it bursts are guaranteed to be `nums[left]` and `nums[right]`.

2. **Array Padding (Sentinels for Out-of-Bounds)**:
  - Create a padded array `padded` of size $n + 2$ with boundary values `padded[0] = 1` and `padded[n + 1] = 1`.
  - This eliminates index out-of-bounds checks and simplifies boundary multiplication logic.

3. **Interval Dynamic Programming**:
  - Define `dp[left][right]` as the maximum coins achievable by bursting all balloons strictly between index `left` and `right`.
  - Iterate over all possible interval lengths $\text{len} = \text{right} - \text{left}$ from $2$ to $n + 1$.
  - For each interval $(left, right)$, iterate through every candidate balloon $k \in (left, right)$ as the last one to burst:
    $$\text{dp}[left][right] = \max_{left < k < right} \Big( \text{padded}[left] \times \text{padded}[k] \times \text{padded}[right] + \text{dp}[left][k] + \text{dp}[k][right] \Big)$$

4. **Complexity**:
  - **Time Complexity**: $O(N^3)$
    - There are $O(N^2)$ intervals, and for each interval, testing every candidate pivot $k$ requires $O(N)$ operations. With $N \le 300$, $N^3 \approx 2.7 \times 10^7$ operations, which easily runs well within typical 1-second execution limits.
  - **Space Complexity**: $O(N^2)$ auxiliary memory
    - Memory allocated for the 2D DP table `dp` of size $(N + 2) \times (N + 2)$.

---

### 💻 Java Solution

```java
/**
- Problem: Burst Balloons (LeetCode 312)
- Language: Java 17
- Time Complexity: O(N^3)
- Space Complexity: O(N^2) auxiliary
- Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;

        // Step 1: Create a padded array with 1 at both boundaries
        int[] padded = new int[n + 2];
        padded[0] = 1;
        padded[n + 1] = 1;
        for (int i = 0; i < n; i++) {
            padded[i + 1] = nums[i];
        }

        // Step 2: DP table where dp[left][right] stores max coins 
        // gained from bursting balloons strictly between index left and right
        int[][] dp = new int[n + 2][n + 2];

        // Step 3: Iterate by interval length (distance between left and right)
        for (int len = 2; len < n + 2; len++) {
            for (int left = 0; left + len < n + 2; left++) {
                int right = left + len;

                // Step 4: Try every balloon k between left and right as the LAST to burst
                for (int k = left + 1; k < right; k++) {
                    int coins = padded[left] * padded[k] * padded[right]
                              + dp[left][k]
                              + dp[k][right];
                              
                    dp[left][right] = Math.max(dp[left][right], coins);
                }
            }
        }

        // The answer for the full open range (0, n + 1)
        return dp[0][n + 1];
    }
}
```