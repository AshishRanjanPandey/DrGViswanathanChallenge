# 🎯 LeetCode 392: Is Subsequence

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given two strings `s` and `t`, return `true` if `s` is a subsequence of `t`, or `false` otherwise.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

#### Examples

- **Example 1**:
  - **Input**: `s = "abc"`, `t = "ahbgdc"`
  - **Output**: `true`

- **Example 2**:
  - **Input**: `s = "axc"`, `t = "ahbgdc"`
  - **Output**: `false`

#### Constraints

- $0 \le s.\text{length} \le 100$
- $0 \le t.\text{length} \le 10^4$
- `s` and `t` consist only of lowercase English letters.

---

### 💡 Intuition & Strategy

1. **Two-Pointer Technique**:
   - We can solve this efficiently by maintaining two pointers: `i` for tracking the current character in string `s`, and `j` for scanning through string `t`.
   - Iterate through `t` using `j`. Whenever the character `t.charAt(j)` matches `s.charAt(i)`, we advance pointer `i` to look for the next character in `s`.
   - Regardless of a match or mismatch, pointer `j` increments on every single step.

2. **Validation**:
   - If pointer `i` successfully reaches the end of `s` (`i == s.length()`), it means all characters of `s` were found in `t` in their correct relative order.

3. **Handling the Follow-Up ($k \ge 10^9$ incoming queries)**:
   - If `t` is static and we receive a massive stream of incoming query strings ($s_1, s_2, \dots, s_k$), scanning `t` repeatedly will cause a Time Limit Exceeded (TLE) error.
   - **Optimization**: Preprocess `t` once by storing the indices of each alphabet character in an array of lists. For each query string `s`, use **Binary Search** (`upper_bound`) to find the next valid occurrence of each character in $t$ in $O(\vert{}s\vert{} \log M)$ time.

4. **Complexity**:
   - **Time Complexity**: $O(N + M)$ for the standard version (where $N$ and $M$ are lengths of `s` and `t`). For the follow-up binary search version, it is $O(\vert{}s\vert{} \log M)$ per query.
   - **Space Complexity**: $O(1)$ auxiliary memory for the standard two-pointer approach ($O(M)$ if using the preprocessing map for the follow-up).

---

### 💻 Java Solution

```java
/**
 * Problem: Is Subsequence (LeetCode 392)
 * Language: Java 17
 * Time Complexity: O(N + M)
 * Space Complexity: O(1)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public boolean isSubsequence(String s, String t) {
        int i = 0, j = 0;
        int n = s.length(), m = t.length();
        
        while (i < n && j < m) {
            if (s.charAt(i) == t.charAt(j)) {
                i++;
            }
            j++;
        }
        
        return i == n;
    }
}
