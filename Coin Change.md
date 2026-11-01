# 🎯 LeetCode 322: Coin Change

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

#### Examples

- **Example 1**:
  - **Input**: `coins = [1,2,5], amount = 11`
  - **Output**: `3`
  - **Explanation**: `11 = 5 + 5 + 1`

- **Example 2**:
  - **Input**: `coins = [2], amount = 3`
  - **Output**: `-1`

- **Example 3**:
  - **Input**: `coins = [1], amount = 0`
  - **Output**: `0`

#### Constraints

- $1 \le \text{coins.length} \le 12$
- $1 \le \text{coins}[i] \le 2^{31} - 1$
- $0 \le \text{amount} \le 10^4$

---

### 💡 Intuition & Strategy

1. **Dynamic Programming (Bottom-Up)**:
   - This is a classic optimization problem that can be broken down into overlapping subproblems. 
   - We can define an array `dp` of size `amount + 1`, where `dp[i]` represents the minimum number of coins required to make the amount `i`.

2. **Initialization & Base Case**:
   - Initialize all elements in the `dp` array to a sentinel value like `amount + 1` (since the maximum possible coins needed would be `amount` if all coins were of denomination 1, `amount + 1` safely acts as "infinity").
   - Set the base case: `dp[0] = 0`, because it takes $0$ coins to make up an amount of $0$.

3. **Building the DP Table**:
   - Iterate through every amount `i` from `1` to `amount`.
   - For each amount `i`, iterate through all available `coins`.
   - If the current coin denomination is less than or equal to `i` (i.e., `i - coin >= 0`), it can be used. Update `dp[i]` by taking the minimum of its current value and `dp[i - coin] + 1` (adding $1$ for the current coin).

4. **Complexity**:
   - **Time Complexity**: $O(S \times N)$
     - Where $S$ is the `amount` and $N$ is the length of the `coins` array. We run an outer loop $S$ times and an inner loop $N$ times.
   - **Space Complexity**: $O(S)$
     - We use an extra array `dp` of size `amount + 1` for memoization.

---

### 💻 Java Solution

```java
/**
 * Problem: Coin Change (LeetCode 322)
 * Language: Java 17
 * Time Complexity: O(S * N) where S is amount, N is coins length
 * Space Complexity: O(S) auxiliary space for DP array
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.Arrays;

class Solution {
    public int coinChange(int[] coins, int amount) {
        // Initialize the dp array with amount + 1, which represents an impossible value
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        
        // Base case: 0 coins are needed to make an amount of 0
        dp[0] = 0;
        
        // Build the dp array from 1 to amount
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                // If the current coin can be used to make the amount i
                if (i - coin >= 0) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        
        // If dp[amount] is still amount + 1, it means the amount cannot be made
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
