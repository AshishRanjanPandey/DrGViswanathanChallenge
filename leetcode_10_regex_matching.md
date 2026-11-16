# 🎯 LeetCode 10: Regular Expression Matching

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `.` and `*` where:
- `.` Matches any single character.
- `*` Matches zero or more of the preceding element.

Return a boolean indicating whether the matching covers the **entire** input string (not partial).

#### Examples

- **Example 1**:
  - **Input**: `s = "aa", p = "a"`
  - **Output**: `false`
  - **Explanation**: "a" does not match the entire string "aa".

- **Example 2**:
  - **Input**: `s = "aa", p = "a*"`
  - **Output**: `true`
  - **Explanation**: `*` means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

- **Example 3**:
  - **Input**: `s = "ab", p = ".*"`
  - **Output**: `true`
  - **Explanation**: `.*` means "zero or more (*) of any character (.)".

#### Constraints

- $1 \le \text{s.length} \le 20$
- $1 \le \text{p.length} \le 20$
- `s` contains only lowercase English letters.
- `p` contains only lowercase English letters, `.`, and `*`.
- It is guaranteed for each appearance of the character `*`, there will be a previous valid character to match.

---

### 💡 Intuition & Strategy

1. **Dynamic Programming Setup**:
   - Create a 2D boolean array `dp` of size $(M + 1) \times (N + 1)$, where $M$ is the length of string `s` and $N$ is the length of pattern `p`.
   - `dp[i][j]` will be `true` if the first `i` characters of `s` match the first `j` characters of `p`.

2. **Base Cases**:
   - `dp[0][0] = true`: An empty string matches an empty pattern.
   - **Empty String against `*` pattern**: A pattern like `a*` or `a*b*` can match an empty string because `*` can represent zero occurrences. We initialize the first row `dp[0][j]` by looking back two steps (`dp[0][j-2]`) whenever we encounter a `*`.

3. **Filling the DP Table**:
   - We iterate through the string and the pattern characters. For each `dp[i][j]`, we have two main cases:
     - **Direct Match or `.`**: If `p[j-1] == s[i-1]` or `p[j-1] == '.'`, they match. The result depends on whether the rest of the string matched the rest of the pattern: `dp[i][j] = dp[i-1][j-1]`.
     - **Handling `*`**: The `*` gives us two options:
       - *Zero occurrences*: We ignore the `*` and its preceding character entirely, pulling the result from `dp[i][j-2]`.
       - *One or more occurrences*: If the preceding character in the pattern matches the current character in the string (or is `.`), we can extend the match by consuming one character of the string, pulling the result from `dp[i-1][j]`.

4. **Complexity**:
   - **Time Complexity**: $O(M \times N)$
     - We iterate through every cell in the $(M + 1) \times (N + 1)$ matrix exactly once, doing constant time $O(1)$ operations inside.
   - **Space Complexity**: $O(M \times N)$
     - We use a 2D array of size $(M + 1) \times (N + 1)$ to store the dynamic programming states.

---

### 💻 Java Solution

```java
/**
 * Problem: Regular Expression Matching (LeetCode 10)
 * Language: Java 17
 * Time Complexity: O(M * N)
 * Space Complexity: O(M * N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length();
        int n = p.length();
        
        // dp[i][j] will be true if s[0..i-1] matches p[0..j-1]
        boolean[][] dp = new boolean[m + 1][n + 1];
        
        // Base case: empty string matches empty pattern
        dp[0][0] = true;
        
        // Handle cases where pattern can match an empty string (e.g., "a*", "a*b*")
        for (int j = 1; j <= n; j++) {
            if (p.charAt(j - 1) == '*') {
                dp[0][j] = dp[0][j - 2];
            }
        }
        
        // Fill the DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                
                // Case 1: Current characters match, or pattern has '.'
                if (p.charAt(j - 1) == s.charAt(i - 1) || p.charAt(j - 1) == '.') {
                    dp[i][j] = dp[i - 1][j - 1];
                } 
                // Case 2: Pattern has '*'
                else if (p.charAt(j - 1) == '*') {
                    
                    // Option A: The '*' represents zero occurrences of the preceding element
                    dp[i][j] = dp[i][j - 2];
                    
                    // Option B: The '*' represents one or more occurrences of the preceding element
                    if (p.charAt(j - 2) == s.charAt(i - 1) || p.charAt(j - 2) == '.') {
                        dp[i][j] = dp[i][j] || dp[i - 1][j];
                    }
                }
            }
        }
        
        return dp[m][n];
    }
}
```