# 🎯 LeetCode 383: Ransom Note

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given two strings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed by using the letters from `magazine` and `false` otherwise.

Each letter in `magazine` can only be used once in `ransomNote`.

#### Examples

- **Example 1**:
  - **Input**: `ransomNote = "a", magazine = "b"`
  - **Output**: `false`

- **Example 2**:
  - **Input**: `ransomNote = "aa", magazine = "ab"`
  - **Output**: `false`

- **Example 3**:
  - **Input**: `ransomNote = "aa", magazine = "aab"`
  - **Output**: `true`

#### Constraints

- $1 \le \text{ransomNote.length}, \text{magazine.length} \le 10^5$
- `ransomNote` and `magazine` consist of lowercase English letters.

---

### 💡 Intuition & Strategy

1. **Lightweight Frequency Map**:
   - Because the problem strictly limits characters to lowercase English letters, a heavy `HashMap` is unnecessary. 
   - An integer array of size $26$ acts as a highly efficient, constant-space frequency map where index $0$ represents 'a', $1$ represents 'b', and so on.

2. **Cataloging the Magazine**:
   - Iterate through the `magazine` string character by character.
   - Use ASCII math (`c - 'a'`) to map each character to its corresponding array index, incrementing the count to record available letters.

3. **Drafting the Note & Validating**:
   - Iterate through the `ransomNote` string.
   - For every letter required, decrement its corresponding index in the frequency array.
   - **The Failsafe**: If at any point an index value drops below $0$, it means the `ransomNote` demands more copies of that letter than the `magazine` possesses. Immediately return `false`.

4. **Complexity**:
   - **Time Complexity**: $O(M + N)$ where $M$ is the length of `magazine` and $N$ is the length of `ransomNote`. We iterate through both strings exactly once. Array index lookups take $O(1)$ time.
   - **Space Complexity**: $O(1)$ auxiliary memory. The `freq` array is always strictly $26$ elements in size regardless of input length, meaning memory scales at a constant rate.

---

### 💻 Java Solution

```java
/**
 * Problem: Ransom Note (LeetCode 383)
 * Language: Java 17
 * Time Complexity: O(M + N)
 * Space Complexity: O(1) auxiliary 
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] freq = new int[26];

        // Count letters in magazine
        for (char c : magazine.toCharArray()) {
            freq[c - 'a']++;
        }

        // Check ransomNote letters
        for (char c : ransomNote.toCharArray()) {
            freq[c - 'a']--;
            if (freq[c - 'a'] < 0) {
                return false;
            }
        }

        return true;
    }
}
```
