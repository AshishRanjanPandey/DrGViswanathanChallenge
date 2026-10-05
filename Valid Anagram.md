# 🎯 LeetCode 242: Valid Anagram

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

#### Examples

- **Example 1**:
  - **Input**: `s = "anagram", t = "nagaram"`
  - **Output**: `true`
  - **Explanation**: Both strings contain the exact same characters with the same frequencies, just in a different order.

- **Example 2**:
  - **Input**: `s = "rat", t = "car"`
  - **Output**: `false`
  - **Explanation**: The strings contain different sets of characters and counts.

#### Constraints

- $1 \le s.\text{length}, t.\text{length} \le 5 \times 10^4$
- `s` and `t` consist of lowercase English letters.

---

### 💡 Intuition & Strategy

1. **Length Pre-check**:
   - If the lengths of `s` and `t` are different, they can never be anagrams. We can immediately return `false` to save execution time.

2. **Frequency Array Mapping**:
   - Since the inputs consist strictly of lowercase English letters (`a-z`), we can allocate a fixed-size integer array of size $26$ (`int[] freq = new int[26]`).
   - We iterate through the strings simultaneously, incrementing the count for characters found in `s` and decrementing the count for characters found in `t`.

3. **Balance Verification**:
   - Finally, we iterate through the `freq` array. If every value is balanced back to `0`, it confirms that every character in `s` has an exact match in `t`. If any non-zero value exists, they are not anagrams.

4. **Complexity**:
   - **Time Complexity**: $\mathcal{O}(N)$
     - Where $N$ is the length of the strings. We traverse the strings once and then perform a constant-time check over the 26-element frequency array.
   - **Space Complexity**: $\mathcal{O}(1)$ auxiliary memory
     - The space used is fixed at size $26$ regardless of the input size.

---

### 💻 Java Solution

```java
/**
 * Problem: Valid Anagram (LeetCode 242)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public boolean isAnagram(String s, String t) {
        // Fast-fail: If lengths differ, they cannot be anagrams
        if (s.length() != t.length()) {
            return false;
        }

        int[] freq = new int[26];

        // Increment for characters in s, decrement for characters in t
        for (int i = 0; i < s.length(); i++) {
            freq[s.charAt(i) - 'a']++;
            freq[t.charAt(i) - 'a']--;
        }

        // Verify all character frequencies balance out to zero
        for (int count : freq) {
            if (count != 0) {
                return false;
            }
        }

        return true;
    }
}
