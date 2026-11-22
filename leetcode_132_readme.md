# 🎯 LeetCode 132: Palindrome Partitioning II

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given a string `s`, partition `s` such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of `s`.

#### Examples

- **Example 1**:
  - **Input**: `s = "aab"`
  - **Output**: `1`
  - **Explanation**: The palindrome partitioning `["aa","b"]` could be produced using $1$ cut.

- **Example 2**:
  - **Input**: `s = "a"`
  - **Output**: `0`
  - **Explanation**: The string is already a palindrome, so no cuts are needed.

- **Example 3**:
  - **Input**: `s = "ab"`
  - **Output**: `1`
  - **Explanation**: Splitting into `["a", "b"]` requires $1$ cut.

#### Constraints

- $1 \le \text{s.length} \le 2000$
- `s` consists of lowercase English letters only.

---

### 💡 Intuition & Strategy

1. **1D Dynamic Programming State**:
   - Define `cuts[i]` as the minimum number of cuts required to partition the substring prefix `s[0..i]`.
   - Initialize `cuts[i] = i` as the worst-case bound (where each character forms its own single-letter palindrome).

2. **Expand Around Center Technique**:
   - Standard 2D DP approaches precalculate a boolean table `isPalindrome[i][j]` in $O(N^2)$ space.
   - We can optimize space to $O(N)$ by treating every index (and gap between indices) as a center and expanding outwards to find palindromes on the fly.
   - For every `center` from `0` to `n - 1`:
     - Expand **odd-length** palindromes with `left = center` and `right = center`.
     - Expand **even-length** palindromes with `left = center` and `right = center + 1`.

3. **Transition Formula**:
   - Whenever `s[left..right]` forms a valid palindrome:
     - If `left == 0`, the prefix `s[0..right]` itself is a palindrome, so `cuts[right] = 0`.
     - If `left > 0`, we can form `s[0..right]` by taking the optimal solution for `s[0..left - 1]` and placing $1$ cut right before index `left`:
       $$\text{cuts}[right] = \min(\text{cuts}[right], \text{cuts}[left - 1] + 1)$$

4. **Complexity**:
   - **Time Complexity**: $O(N^2)$
     - There are $2N - 1$ potential palindrome centers. Expanding each center takes at most $O(N)$ steps.
   - **Space Complexity**: $O(N)$ auxiliary memory
     - Only requires a 1D `cuts` array of size $N$, avoiding $O(N^2)$ matrix memory.

---

### 💻 Java Solution

```java
/**
 * Problem: Palindrome Partitioning II (LeetCode 132)
 * Language: Java 17
 * Time Complexity: O(N^2)
 * Space Complexity: O(N) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int minCut(String s) {
        int n = s.length();
        int[] cuts = new int[n];

        // Worst case: i cuts needed for prefix s[0..i]
        for (int i = 0; i < n; i++) {
            cuts[i] = i;
        }

        for (int center = 0; center < n; center++) {
            // Odd-length palindromes centered at `center`
            expand(s, center, center, cuts);
            // Even-length palindromes centered between `center` and `center + 1`
            expand(s, center, center + 1, cuts);
        }

        return cuts[n - 1];
    }

    private void expand(String s, int left, int right, int[] cuts) {
        int n = s.length();
        while (left >= 0 && right < n && s.charAt(left) == s.charAt(right)) {
            // If the palindrome starts at index 0, 0 cuts are needed for s[0..right]
            int newCut = (left == 0) ? 0 : cuts[left - 1] + 1;
            cuts[right] = Math.min(cuts[right], newCut);
            left--;
            right++;
        }
    }
}
```