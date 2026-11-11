# 🎯 LeetCode 139: Word Break

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

Notice that the same word in the dictionary may be reused multiple times in the segmentation.

#### Examples

- **Example 1**:
  - **Input**: `s = "leetcode"`, `wordDict = ["leet","code"]`
  - **Output**: `true`
  - **Explanation**: Return `true` because `"leetcode"` can be segmented as `"leet code"`.

- **Example 2**:
  - **Input**: `s = "applepenapple"`, `wordDict = ["apple","pen"]`
  - **Output**: `true`
  - **Explanation**: Return `true` because `"applepenapple"` can be segmented as `"apple pen apple"`. Note that you are allowed to reuse a dictionary word.

- **Example 3**:
  - **Input**: `s = "catsandog"`, `wordDict = ["cats","dog","sand","and","cat"]`
  - **Output**: `false`

#### Constraints

- $1 \le \text{s.length} \le 300$
- $1 \le \text{wordDict.length} \le 1000$
- $1 \le \text{wordDict}[i]\text{.length} \le 20$
- `s` and `wordDict[i]` consist of only lowercase English letters.
- All the strings of `wordDict` are unique.

---

### 💡 Intuition & Strategy

1. **Dynamic Programming State Definition**:
   - Define a boolean DP array `dp` of size $N + 1$, where $N = \text{s.length()}$.
   - `dp[i]` represents whether the prefix substring `s[0...i-1]` can be segmented into valid words from `wordDict`.

2. **Base Case & State Transition**:
   - **Base Case**: `dp[0] = true` because an empty prefix is trivially segmentable.
   - **Transition**: For each end index `i` from `1` to $N$, evaluate every split position `j` from `0` to `i - 1`.
   - If `dp[j]` is `true` (meaning the prefix `s[0...j-1]` is valid) **AND** the substring `s[j...i-1]` exists in `wordDict`, set `dp[i] = true`.
   - Once `dp[i]` is found to be `true`, `break` early out of the inner loop to optimize iterations.

3. **HashSet Lookup Optimization**:
   - Store `wordDict` in a `HashSet` to enable constant-time $O(1)$ average lookup for dictionary membership.

4. **Complexity**:
   - **Time Complexity**: $O(N^2 \cdot L)$
     - Outer loop runs $N$ times and inner loop runs up to $N$ times.
     - Extracting substring `s.substring(j, i)` and checking `HashSet` containment takes $O(L)$ time, where $L$ is the max length of words in `wordDict` ($L \le 20$). Given $N \le 300$, total operations are well within the execution time limit.
   - **Space Complexity**: $O(N + M)$ auxiliary memory
     - $O(N)$ for the DP array of size $N + 1$.
     - $O(M)$ for storing the dictionary words in the `HashSet`, where $M$ is the total character length across all words in `wordDict`.

---

### 💻 Java Solution

```java
/**
 * Problem: Word Break (LeetCode 139)
 * Language: Java 17
 * Time Complexity: O(N^2 * L)
 * Space Complexity: O(N + M) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        // Store dictionary words in a HashSet for fast lookups
        Set<String> wordSet = new HashSet<>(wordDict);
        
        // dp[i] represents whether s[0...i-1] can be segmented
        boolean[] dp = new boolean[s.length() + 1];
        
        // Base case: empty string can always be segmented
        dp[0] = true;

        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                // If prefix s[0...j-1] is valid and remaining substring s[j...i-1] is in dictionary
                if (dp[j] && wordSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break; // Skip further split checks for this i
                }
            }
        }

        return dp[s.length()];
    }
}
```