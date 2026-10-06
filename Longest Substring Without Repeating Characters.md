# 🎯 LeetCode 3: Longest Substring Without Repeating Characters

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given a string `s`, find the length of the longest substring without duplicate characters.

#### Examples

- **Example 1**:
  - **Input**: `s = "abcabcbb"`
  - **Output**: `3`
  - **Explanation**: The answer is `"abc"`, with the length of 3. (Note that `"bca"` and `"cab"` are also correct answers).

- **Example 2**:
  - **Input**: `s = "bbbbb"`
  - **Output**: `1`
  - **Explanation**: The answer is `"b"`, with the length of 1.

- **Example 3**:
  - **Input**: `s = "pwwkew"`
  - **Output**: `3`
  - **Explanation**: The answer is `"wke"`, with the length of 3. Notice that the answer must be a substring, `"pwke"` is a subsequence and not a substring.

#### Constraints

- $0 \le s.\text{length} \le 10^5$
- `s` consists of English letters, digits, symbols and spaces.

---

### 💡 Intuition & Strategy

1. **Sliding Window Pattern**:
   - We use two pointers, `left` and `right`, to define a dynamic window representing the current substring without repeating characters.
   - The `right` pointer expands the window by iterating through the string character by character.

2. **Character Tracking with HashMap**:
   - A `HashMap` stores each character alongside its most recent index. This allows us to look up previous occurrences instantly.

3. **Smart Window Adjustment**:
   - When we encounter a character that already exists in our map **and** its previous index lies within our current active window (`map.get(currentChar) >= left`), we instantly shift the `left` pointer to `map.get(currentChar) + 1`. This skips past the old duplicate without redundant nested loops.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - The `right` pointer traverses the string of length $N$ exactly once. Hash map operations take constant time on average.
   - **Space Complexity**: $O(\min(N, M))$ 
     - Where $M$ is the size of the character set (e.g., 256 for ASCII). In the worst case, the hash map stores all unique characters.

---

### 💻 Java Solution

```java
/**
 * Problem: Longest Substring Without Repeating Characters (LeetCode 3)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(min(N, M)) where M is the charset size
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        Map<Character, Integer> map = new HashMap<>();
        int maxLength = 0;
        int left = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char currentChar = s.charAt(right);
            
            // If the character is already in the map and its index is inside the current window
            if (map.containsKey(currentChar) && map.get(currentChar) >= left) {
                left = map.get(currentChar) + 1;
            }
            
            // Update the last seen position of the character
            map.put(currentChar, right);
            
            // Calculate the maximum length found so far
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}
