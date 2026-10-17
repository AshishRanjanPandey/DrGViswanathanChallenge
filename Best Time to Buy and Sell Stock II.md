# 🎯 LeetCode 122: Best Time to Buy and Sell Stock II

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the $i^{\text{th}}$ day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can sell and buy the stock multiple times on the same day, ensuring you never hold more than one share of the stock.

Find and return the maximum profit you can achieve.

#### Examples

- **Example 1**:
  - **Input**: `prices = [7,1,5,3,6,4]`
  - **Output**: `7`
  - **Explanation**: 
    - Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = $5 - 1 = 4$.
    - Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = $6 - 3 = 3$.
    - Total profit is $4 + 3 = 7$.

- **Example 2**:
  - **Input**: `prices = [1,2,3,4,5]`
  - **Output**: `4`
  - **Explanation**: 
    - Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = $5 - 1 = 4$.
    - Total profit is $4$.

- **Example 3**:
  - **Input**: `prices = [7,6,4,3,1]`
  - **Output**: `0`
  - **Explanation**: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of $0$.

#### Constraints

- $1 \le \text{prices.length} \le 3 \times 10^4$
- $0 \le \text{prices}[i] \le 10^4$

---

### 💡 Intuition & Strategy

1. **Greedy Valley-to-Peak Approach**:
   - Since you can buy and sell multiple times and even complete transactions on the same day, you can capture every single upward price movement (every positive slope).
   - Whenever the price on day $i$ is greater than the price on day $i - 1$, we add the difference `prices[i] - prices[i - 1]` to our total profit.

2. **Complexity**:
   - **Time Complexity**: $O(N)$
     - A single pass through the `prices` array of length $N$.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Only a single integer variable (`maxProfit`) is maintained.

---

### 💻 Java Solution

```java
/**
 * Problem: Best Time to Buy and Sell Stock II (LeetCode 122)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        
        // Iterate through the prices starting from the second day
        for (int i = 1; i < prices.length; i++) {
            // Capture all upward price movements (greedy valley-to-peak)
            if (prices[i] > prices[i - 1]) {
                maxProfit += prices[i] - prices[i - 1];
            }
        }
        
        return maxProfit;
    }
}
