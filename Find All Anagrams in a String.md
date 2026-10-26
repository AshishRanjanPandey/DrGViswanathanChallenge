# 🎯 LeetCode 438: Find All Anagrams in a String

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`. You may return the answer in any order.

#### Examples

- **Example 1**:
  - **Input**: `s = "cbaebabacd"`, `p = "abc"`
  - **Output**: `[0,6]`
  - **Explanation**: 
    - The substring with start index = 0 is "cba", which is an anagram of "abc".
    - The substring with start index = 6 is "bac", which is an anagram of "abc".

- **Example 2**:
  - **Input**: `s = "abab"`, `p = "ab"`
  - **Output**: `[0,1,2]`
  - **Explanation**:
    - The substring with start index = 0 is "ab", which is an anagram of "ab".
    - The substring with start index = 1 is "ba", which is an anagram of "ab".
    - The substring with start index = 2 is "ab", which is an anagram of "ab".

#### Constraints

- $1 \le \text{s.length}, \text{p.length} \le 3 \times 10^4$
- `s` and `p` consist of lowercase English letters.

---

### 💡 Intuition & Strategy

1. **Character Frequency Arrays**:
   - Since both strings only contain lowercase English letters, we can use integer arrays of size 26 instead of `HashMap` objects. This provides much faster lookups and updates with minimal memory overhead.

2. **The Sliding Window Technique**:
   - We need to find substrings in `s` that match the length of `p`. By sliding a window of size `p.length()` across string `s`, we can evaluate one substring at a time.
   - For every step, we add the new character entering the right side of our window to our frequency count (`sCount`).

3. **Maintaining the Window**:
   - If our current index `i` is greater than or equal to the window size, it means our window has grown too large. We must decrement the count of the character that is left behind on the left side of the window.
   - Once the window reaches the exact length of `p` (at index `i >= p.length() - 1`), we compare our window's frequency array (`sCount`) with the target array (`pCount`). If they match exactly, we've found an anagram and record its starting index.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - We iterate through string `s` exactly once, taking $O(N)$ time. The comparison of two arrays of size 26 takes $O(1)$ time per iteration, resulting in an overall linear time complexity.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Beyond the list used for the output, we only allocate two integer arrays of fixed size (26), which takes constant space regardless of the input size.

---

### 💻 Java Solution

```java
/**
 * Problem: Find All Anagrams in a String (LeetCode 438)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1) auxiliary (excluding output)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        
        // Edge case: if s is shorter than p, no anagrams can exist
        if (s == null || s.length() == 0 || s.length() < p.length()) {
            return result;
        }
        
        int[] pCount = new int[26];
        int[] sCount = new int[26];
        
        // Populate the frequency array for target string p
        for (char c : p.toCharArray()) {
            pCount[c - 'a']++;
        }
        
        int windowSize = p.length();
        
        // Slide a window of size windowSize across string s
        for (int i = 0; i < s.length(); i++) {
            // Add the new character to the current window
            sCount[s.charAt(i) - 'a']++;
            
            // Remove the character that is left behind as the window slides
            if (i >= windowSize) {
                sCount[s.charAt(i - windowSize) - 'a']--;
            }
            
            // Compare the frequency arrays when the window size matches p's length
            if (i >= windowSize - 1) {
                if (Arrays.equals(pCount, sCount)) {
                    result.add(i - windowSize + 1);
                }
            }
        }
        
        return result;
    }
}
