# 🎯 LeetCode 76: Minimum Window Substring

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given two strings `s` and `t` of lengths `m` and `n` respectively, return the **minimum window substring** of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.

The testcases will be generated such that the answer is unique.

#### Examples

- **Example 1**:
  - **Input**: `s = "ADOBECODEBANC"`, `t = "ABC"`
  - **Output**: `"BANC"`
  - **Explanation**: The minimum window substring `"BANC"` includes `'A'`, `'B'`, and `'C'` from string `t`.

- **Example 2**:
  - **Input**: `s = "a"`, `t = "a"`
  - **Output**: `"a"`
  - **Explanation**: The entire string `s` is the minimum window.

- **Example 3**:
  - **Input**: `s = "a"`, `t = "aa"`
  - **Output**: `""`
  - **Explanation**: Both `'a'`s from `t` must be included in the window. Since the largest window of `s` only has one `'a'`, return empty string.

#### Constraints

- $m == \text{s.length}$
- $n == \text{t.length}$
- $1 \le m, n \le 10^5$
- `s` and `t` consist of uppercase and lowercase English letters.

---

### 💡 Intuition & Strategy

1. **Character Frequency Mapping**:
   - Use an integer array of size $128$ (to cover all ASCII characters) as a frequency map to keep track of the characters required from string `t`.
   - Iterate through `t` and increment the count for each character.

2. **Sliding Window Technique (Expand & Contract)**:
   - Use two pointers, `left` and `right`, both starting at $0$.
   - **Expand**: Move the `right` pointer to expand the window. For each character `s.charAt(right)`, decrement its count in the frequency map. If the count in the map was strictly greater than $0$ before decrementing, it means a required character from `t` was found, so decrease a global `count` variable (initialized to the length of `t`).
   - **Contract**: When `count == 0` (meaning all characters from `t` are currently in the window), try to shrink the window from the `left` to find the *minimum* length.
   - Update the minimum window parameters (`minLen` and `minLeft`) if the current window is smaller than previously recorded ones.

3. **Restoring State**:
   - While contracting, the character at the `left` pointer is being removed from the window. Increment its value in the frequency map. If its value becomes strictly greater than $0$, it means a required character was just removed, so increment the global `count` variable (breaking the `while(count == 0)` loop and forcing the `right` pointer to expand again).

4. **Complexity**:
   - **Time Complexity**: $O(M + N)$
     - Building the frequency map takes $O(N)$ where $N$ is the length of `t`.
     - The `right` and `left` pointers traverse string `s` (length $M$) at most once, taking $O(M)$ time.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - The frequency map size is fixed at $128$ integers, which requires constant $O(1)$ space regardless of the input sizes.

---

### 💻 Java Solution

```java
/**
 * Problem: Minimum Window Substring (LeetCode 76)
 * Language: Java
 * Time Complexity: O(M + N)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public String minWindow(String s, String t) {
        if (s == null || s.length() == 0 || t == null || t.length() == 0) {
            return "";
        }

        // Frequency map for characters in t
        int[] map = new int[128];
        for (char c : t.toCharArray()) {
            map[c]++;
        }

        int left = 0, right = 0; 
        int minLeft = 0, minLen = Integer.MAX_VALUE;
        int count = t.length();

        while (right < s.length()) {
            // Expand the window by moving the right pointer
            if (map[s.charAt(right++)]-- > 0) {
                count--;
            }

            // When all characters are found, try shrinking the window from the left
            while (count == 0) {
                // Update the minimum window length and starting index
                if (right - left < minLen) {
                    minLen = right - left;
                    minLeft = left;
                }

                // Remove the left character from the window and update the map
                if (map[s.charAt(left++)]++ == 0) {
                    count++;
                }
            }
        }

        return minLen == Integer.MAX_VALUE ? "" : s.substring(minLeft, minLeft + minLen);
    }
}
```