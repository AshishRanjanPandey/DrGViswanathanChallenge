# 🎯 LeetCode 62: Unique Paths

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

There is a robot on an $m \times n$ grid. The robot is initially located at the top-left corner (i.e., `grid[0][0]`). The robot tries to move to the bottom-right corner (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to $2 \times 10^9$.

#### Examples

- **Example 1**:
  - **Input**: `m = 3, n = 7`
  - **Output**: `28`

- **Example 2**:
  - **Input**: `m = 3, n = 2`
  - **Output**: `3`
  - **Explanation**: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
    1. `Right -> Down -> Down`
    2. `Down -> Down -> Right`
    3. `Down -> Right -> Down`

#### Constraints

- $1 \le m, n \le 100$

---

### 💡 Intuition & Strategy

1. **Mathematical Formulation (Combinatorics)**:
   - To travel from `grid[0][0]` to `grid[m - 1][n - 1]`, the robot must make a fixed number of directional steps:
     - Exactly $m - 1$ steps **Down** ($D$)
     - Exactly $n - 1$ steps **Right** ($R$)
   - Total number of steps required: $N = (m - 1) + (n - 1) = m + n - 2$.
   - Any path is uniquely defined by choosing which of the $N$ total steps are **Down** (or equivalently, **Right**).
   - This translates directly to the combinations formula:
     $$\binom{m + n - 2}{m - 1} = \frac{(m + n - 2)!}{(m - 1)! \cdot (n - 1)!}$$

2. **Symmetry & Overflow Handling**:
   - By identity, $\binom{N}{k} = \binom{N}{N - k}$. Setting $k = \min(m - 1, n - 1)$ minimizes the number of loop iterations to at most $\min(m, n) - 1$.
   - Directly calculating factorials leads to integer overflow even for modest values of $m$ and $n$.
   - We compute the binomial coefficient iteratively using a 64-bit integer (`long`):
     $$\text{ans} = \text{ans} \times \frac{N - k + i}{i} \quad \text{for } i \in [1, k]$$
   - Performing multiplication before division at each step ensures exact integer division because the product of any $i$ consecutive integers is always divisible by $i!$.

3. **Alternative: Dynamic Programming (Memoization / Tabulation)**:
   - Let `dp[j]` denote the number of unique paths to cell `(i, j)`.
   - Transition: `dp[j] = dp[j] + dp[j - 1]` (sum of paths from the cell above and the cell to the left).
   - Achieves $O(m \times n)$ time and $O(n)$ auxiliary space. Combinatorics is chosen below for optimal $O(\min(m, n))$ time and $O(1)$ space.

4. **Complexity**:
   - **Time Complexity**: $O(\min(m, n))$
     - The loop executes at most $\min(m - 1, n - 1)$ iterations.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Only scalar variables (`long ans`, `int totalMoves`, `int k`) are maintained.

---

### 💻 Java Solution

```java
/**
 * Problem: Unique Paths (LeetCode 62)
 * Language: Java 17
 * Time Complexity: O(min(m, n))
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int uniquePaths(int m, int n) {
        int totalMoves = m + n - 2;
        int k = Math.min(m - 1, n - 1); // C(N, k) == C(N, N - k)

        long ans = 1;

        // Iteratively compute C(totalMoves, k)
        for (int i = 1; i <= k; i++) {
            ans = ans * (totalMoves - k + i) / i;
        }

        return (int) ans;
    }
}
