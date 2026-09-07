# 🎯 LeetCode 49: Group Anagrams

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

An **anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

#### Examples

- **Example 1**:
  - **Input**: `strs = ["eat", "tea", "tan", "ate", "nat", "bat"]`
  - **Output**: `[["bat"], ["nat", "tan"], ["ate", "eat", "tea"]]`
  - **Explanation**:
    - There is no string in `strs` that can be rearranged to form `"bat"`.
    - The strings `"nat"` and `"tan"` are anagrams as they can be rearranged to form each other.
    - The strings `"ate"`, `"eat"`, and `"tea"` are anagrams as they can be rearranged to form each other.

- **Example 2**:
  - **Input**: `strs = [""]`
  - **Output**: `[[""]]`

- **Example 3**:
  - **Input**: `strs = ["a"]`
  - **Output**: `[["a"]]`

#### Constraints

- $1 \le \text{strs.length} \le 10^4$
- $0 \le \text{strs}[i]\text{.length} \le 100$
- `strs[i]` consists of lowercase English letters.

---

### 💡 Intuition & Strategy

1. **Character Invariance Principle**:
   - Anagrams share identical character frequencies regardless of initial permutation.
   - To aggregate anagrams together, we need a canonical representation (a normalized "key") that maps every anagram variant to a single bucket in a hash table.

2. **Categorization via Canonical Keys**:
   - **Sorting Strategy**:
     - Converting a string to a character array and sorting it alphabetizes the letters (e.g., `"eat"`, `"tea"`, and `"ate"` all yield `"aet"`).
     - The sorted string acts as a deterministic hash map key: `Map<String, List<String>>`.
   - **Frequency Counting Strategy**:
     - Given only lowercase English letters ($26$ possible values), each string can also be mapped using a frequency vector of length $26$ (e.g., `#1#0...#1`).

3. **Hash Map Grouping**:
   - Iterate over each word in `strs`.
   - Compute its canonical sorted key.
   - Check the map using `map.putIfAbsent(key, new ArrayList<>())` and append the original word.
   - Wrap and return the collection of values: `new ArrayList<>(map.values())`.

4. **Complexity**:
   - **Time Complexity**: $O(N \cdot K \log K)$, where $N$ is the number of strings and $K$ is the maximum length of a string. Sorting each word takes $O(K \log K)$, and inserting into the hash table takes $O(K)$ on average.
   - **Space Complexity**: $O(N \cdot K)$ to store the strings and sorted keys inside the hash map.

---

### 💻 Java Solution

```java
/**
 * Problem: Group Anagrams (LeetCode 49)
 * Language: Java 17
 * Time Complexity: O(N * K log K)
 * Space Complexity: O(N * K)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {

    public List<List<String>> groupAnagrams(String[] strs) {
        // Map canonical sorted strings to lists of anagrams
        Map<String, List<String>> map = new HashMap<>();

        for (String s : strs) {
            // Convert to char array and sort to generate canonical signature
            char[] ch = s.toCharArray();
            Arrays.sort(ch);
            String key = new String(ch);

            // Initialize bucket if missing and insert original string
            map.putIfAbsent(key, new ArrayList<>());
            map.get(key).add(s);
        }

        // Return aggregated anagram groups
        return new ArrayList<>(map.values());
    }
}
