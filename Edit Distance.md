# 🎯 LeetCode 72: Edit Distance

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`.

You have the following three operations permitted on a word:
- Insert a character
- Delete a character
- Replace a character

#### Examples

- **Example 1**:
  - **Input**: `word1 = "horse"`, `word2 = "ros"`
  - **Output**: `3`
  - **Explanation**: 
    - `horse -> rorse` (replace 'h' with 'r')
    - `rorse -> rose` (remove 'r')
    - `rose -> ros` (remove 'e')

- **Example 2**:
  - **Input**: `word1 = "intention"`, `word2 = "execution"`
  - **Output**: `5`
  - **Explanation**: 
    - `intention -> inention` (remove 't')
    - `inention -> enention` (replace 'i' with 'e')
    - `enention -> exention` (replace 'n' with 'x')
    - `exention -> exection` (replace 'n' with 'c')
    - `exection -> execution` (insert 'u')

#### Constraints

- $0 \le \text{word1.length}, \text{word2.length} \le 500$
- `word1` and `word2` consist of lowercase English letters.

---

### 💡 Intuition & Strategy

1. **Dynamic Programming 2D Grid**:
   - We can break this problem down into smaller subproblems. Let `dp[i][j]` represent the minimum number of operations required to convert the first `i` characters of `word1` to the first `j` characters of `word2`.
   - We build a 2D array of size $(M + 1) \times (N + 1)$, where $M$ is the length of `word1` and $N$ is the length of `word2`.

2. **Base Cases**:
   - If `word1` is empty, the only way to form `word2` is to **insert** all of its characters. Thus, `dp[0][j] = j`.
   - If `word2` is empty, the only way to convert `word1` into it is to **delete** all of its characters. Thus, `dp[i][0] = i`.

3. **State Transition**:
   - Iterate through the strings. For each pair of characters at `word1[i-1]` and `word2[j-1]`:
     - **If they match**: No new operations are needed. The cost is the same as the prefixes before these characters: `dp[i][j] = dp[i - 1][j - 1]`.
     - **If they don't match**: We must perform one operation. We take the minimum cost among three choices and add $1$:
       1. **Replace**: Look at `dp[i - 1][j - 1]`
       2. **Delete**: Look at `dp[i - 1][j]` (removing a character from `word1`)
       3. **Insert**: Look at `dp[i][j - 1]` (adding a character to match `word2`)

4. **Complexity**:
   - **Time Complexity**: $O(M \times N)$
     - We use nested loops to iterate through a grid of size $M \times N$, doing a constant $O(1)$ amount of work at each step.
   - **Space Complexity**: $O(M \times N)$
     - We require an auxiliary 2D matrix of size $(M + 1) \times (N + 1)$ to store intermediate computations. *(Note: This can be optimized to $O(N)$ by only storing the current and previous rows).*

---

### 💻 Java Solution

```java
/**
 * Problem: Edit Distance (LeetCode 72)
 * Language: Java
 * Time Complexity: O(M * N)
 * Space Complexity: O(M * N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        
        // dp[i][j] represents the minimum operations to convert 
        // the first i characters of word1 to the first j characters of word2
        int[][] dp = new int[m + 1][n + 1];
        
        // Base case: If word2 is empty, we must delete all characters from word1
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        
        // Base case: If word1 is empty, we must insert all characters of word2
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }
        
        // Fill the DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // If characters match, no operation is needed for this step
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // If they don't match, take the minimum of 3 operations + 1
                    dp[i][j] = 1 + Math.min(
                        dp[i - 1][j - 1], // Replace
                        Math.min(
                            dp[i - 1][j], // Delete from word1
                            dp[i][j - 1]  // Insert into word1
                        )
                    );
                }
            }
        }
        
        return dp[m][n];
    }
}
```
