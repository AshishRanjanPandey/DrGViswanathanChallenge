# 🎯 LeetCode 91: Decode Ways

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

A message containing letters from `A-Z` can be encoded into numbers using the following mapping:

- `"1" -> 'A'`
- `"2" -> 'B'`
- ...
- `"25" -> 'Y'`
- `"26" -> 'Z'`

To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, `"11106"` can be decoded into:
- `"AAJF"` with the grouping `(1, 1, 10, 6)`
- `"KJF"` with the grouping `(11, 10, 6)`
- The grouping `(1, 11, 06)` is invalid because `"06"` is not a valid code (only `"6"` is valid).

Note that there may be strings that are impossible to decode.

Given a string `s` containing only digits, return the number of ways to decode it. If the entire string cannot be decoded in any valid way, return `0`.

The test cases are generated so that the answer fits in a 32-bit integer.

#### Examples

- **Example 1**:
  - **Input**: `s = "12"`
  - **Output**: `2`
  - **Explanation**: `"12"` could be decoded as `"AB"` ($1, 2$) or `"L"` ($12$).

- **Example 2**:
  - **Input**: `s = "226"`
  - **Output**: `3`
  - **Explanation**: `"226"` could be decoded as `"BZ"` ($2, 26$), `"VF"` ($22, 6$), or `"BBF"` ($2, 2, 6$).

- **Example 3**:
  - **Input**: `s = "06"`
  - **Output**: `0`
  - **Explanation**: `"06"` cannot be mapped to `"F"` because of the leading zero (`"6"` is different from `"06"`). In this case, the string is not a valid encoding, so return `0`.

#### Constraints

- $1 \le s.\text{length} \le 100$
- `s` contains only digits and may contain leading zero(s).

---

### 💡 Intuition & Strategy

1. **Dynamic Programming Subproblem Structure**:
   - Let `dp[i]` represent the number of ways to decode the substring of length `i` (i.e., prefix `s[0...i-1]`).
   - The length of the `dp` array will be `n + 1`, where `n` is the length of the string `s`.

2. **Base Cases**:
   - `dp[0] = 1`: An empty string has 1 valid decoding way.
   - `dp[1]`: If the first character is `'0'`, there are $0$ ways to decode it; otherwise, there is $1$ way.

3. **Transition States**:
   - For each index `i` from `2` to `n`:
     - **Single Digit Check ($1$-digit)**: Look at the single character `s[i-1]`. If it is between `'1'` and `'9'`, it can form a valid letter by itself, so we add `dp[i - 1]` to `dp[i]`.
     - **Two Digit Check ($2$-digit)**: Look at the substring of length 2 (`s[i-2...i-1]`). If the parsed integer value is between `10` and `26`, it forms a valid letter combination, so we add `dp[i - 2]` to `dp[i]`.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - We perform a single linear scan from index $2$ to $n$, executing constant time checks at each step.
   - **Space Complexity**: $O(N)$ auxiliary memory for the DP table (which can optionally be optimized down to $O(1)$ by tracking just the last two state variables).

---

### 💻 Java Solution

```java
/**
 * Problem: Decode Ways (LeetCode 91)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
public class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        int n = s.length();
        // dp[i] represents the number of ways to decode the substring s[0...i-1]
        int[] dp = new int[n + 1];
        
        dp[0] = 1; // Base case: an empty string has 1 way to be decoded
        dp[1] = s.charAt(0) != '0' ? 1 : 0;

        for (int i = 2; i <= n; i++) {
            // Check single digit (current character)
            int oneDigit = Integer.parseInt(s.substring(i - 1, i));
            if (oneDigit >= 1 && oneDigit <= 9) {
                dp[i] += dp[i - 1];
            }

            // Check two digits (previous and current character)
            int twoDigits = Integer.parseInt(s.substring(i - 2, i));
            if (twoDigits >= 10 && twoDigits <= 26) {
                dp[i] += dp[i - 2];
            }
        }

        return dp[n];
    }
}
```
