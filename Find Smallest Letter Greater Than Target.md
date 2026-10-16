# 🎯 LeetCode 744: Find Smallest Letter Greater Than Target

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given a characters array `letters` that is sorted in non-decreasing order and a character `target`, return the smallest character in `letters` that is lexicographically greater than `target`. If such a character does not exist, return the first character in `letters`.

#### Examples

- **Example 1**:
  - **Input**: `letters = ["c","f","j"]`, `target = "a"`
  - **Output**: `"c"`
  - **Explanation**: The smallest character that is lexicographically greater than `'a'` in `letters` is `'c'`.

- **Example 2**:
  - **Input**: `letters = ["c","f","j"]`, `target = "c"`
  - **Output**: `"f"`
  - **Explanation**: The smallest character that is lexicographically greater than `'c'` in `letters` is `'f'`.

- **Example 3**:
  - **Input**: `letters = ["x","x","y","y"]`, `target = "z"`
  - **Output**: `"x"`
  - **Explanation**: There are no characters in `letters` that are lexicographically greater than `'z'`, so we return `letters[0]`.

#### Constraints

- $2 \le \text{letters.length} \le 10^4$
- `letters[i]` is a lowercase English letter.
- `letters` is sorted in non-decreasing order.
- `letters` contains at least two different characters.
- `target` is a lowercase English letter.

---

### 💡 Intuition & Strategy

1. **Leveraging Sorted Property with Binary Search**:
   - Since the array is sorted in non-decreasing order, we can achieve $O(\log n)$ time complexity using binary search instead of scanning linearly.

2. **Maintaining the Search Range**:
   - Initialize `left = 0` and `right = letters.length - 1`.
   - Calculate the midpoint `mid = left + (right - left) / 2`.
   - Compare `letters[mid]` with `target`:
     - If `letters[mid] <= target`: The target character must lie further to the right. Move `left = mid + 1`.
     - If `letters[mid] > target`: We found a potential candidate, but a smaller valid character might exist to the left. Record the choice and move `right = mid - 1`.

3. **Handling Wrap-Around**:
   - If no character is strictly greater than `target`, the `left` pointer will end up pointing past the end of the array (`letters.length`). Using `left % letters.length` gracefully wraps around to return `letters[0]`.

4. **Complexity**:
   - **Time Complexity**: $O(\log n)$ — Binary search halves the search space at each iteration.
   - **Space Complexity**: $O(1)$ — Only a few pointer variables are used, requiring constant auxiliary memory.

---

### 💻 Java Solution

```java
/**
 * Problem: Find Smallest Letter Greater Than Target (LeetCode 744)
 * Language: Java 17
 * Time Complexity: O(log N)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int left = 0;
        int right = letters.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (letters[mid] <= target) {
                // If the middle character is less than or equal to target,
                // the answer must be to the right.
                left = mid + 1;
            } else {
                // If the middle character is strictly greater than target,
                // look further left for a smaller one.
                right = mid - 1;
            }
        }
        
        // If 'left' exceeds the array bounds, wrap around to return the first element.
        return letters[left % letters.length];
    }
}
