# 🎯 LeetCode 1: Two Sum

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an array of integers `nums` and an integer `target`, return **indices of the two numbers** such that they add up to `target`.

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

You can return the answer in any order.

#### Examples

- **Example 1**:
  - **Input**: `nums = [2, 7, 11, 15]`, `target = 9`
  - **Output**: `[0, 1]`
  - **Explanation**: Because `nums[0] + nums[1] == 9`, we return `[0, 1]`.

- **Example 2**:
  - **Input**: `nums = [3, 2, 4]`, `target = 6`
  - **Output**: `[1, 2]`

- **Example 3**:
  - **Input**: `nums = [3, 3]`, `target = 6`
  - **Output**: `[0, 1]`

#### Constraints

- $2 \le \text{nums.length} \le 10^4$
- $-10^9 \le \text{nums}[i] \le 10^9$
- $-10^9 \le \text{target} \le 10^9$
- **Only one valid answer exists.**

---

### 💡 Intuition & Strategy

1. **The Brute Force Bottleneck**:
   - A naive approach tests all pairs $(i, j)$ where $i < j$ to see if $\text{nums}[i] + \text{nums}[j] = \text{target}$.
   - This requires nested loops running in $O(n^2)$ time, which becomes inefficient as the input size reaches $10^4$ elements.

2. **Complement Lookup via Hash Map**:
   - For every number $x = \text{nums}[i]$, the required matching number to hit the target is:
     $$\text{complement} = \text{target} - x$$
   - By utilizing a Hash Map (`unordered_map` / `HashMap`), we can record elements and their corresponding indices as we traverse the array.
   - Hash maps provide average $O(1)$ constant time lookup, allowing us to immediately check whether the needed complement has already been observed.

3. **One-Pass Optimization**:
   - Rather than populating the entire map first and searching in a second pass, we can inspect and insert elements in a single forward pass:
     1. Compute $\text{complement} = \text{target} - \text{nums}[i]$.
     2. If `complement` already exists in the map, return the pair of indices: `[map.get(complement), i]`.
     3. Otherwise, map the current value to its index: `map.put(nums[i], i)`.
   - This avoids self-matching (using the same element twice) automatically, because the current element is not added to the map until after checking for its complement.

4. **Complexity**:
   - **Time Complexity**: $O(n)$ — We traverse the array of length $n$ exactly once. Each map lookup and insertion runs in amortized $O(1)$ time.
   - **Space Complexity**: $O(n)$ — In the worst-case scenario, the map stores up to $n - 1$ key-value pairs before locating the valid complement.

---

### 💻 Java Solution

```java
/**
 * Problem: Two Sum (LeetCode 1)
 * Language: Java 17
 * Time Complexity: O(n)
 * Space Complexity: O(n)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.HashMap;
import java.util.Map;

class Solution {

    public int[] twoSum(int[] nums, int target) {
        // Map to store: key = number value, value = index in array
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];

            // If complement exists in map, we found the unique pair
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }

            // Store current number and its index for subsequent lookups
            map.put(nums[i], i);
        }

        // Guaranteed to have a solution per constraints, fallback exception
        throw new IllegalArgumentException("No two sum solution found");
    }
}
```
### ⏱️ Complexity Analysis

| Approach | Time Complexity | Space Complexity | Notes |
| :--- | :---: | :---: | :--- |
| **Brute Force** | $O(n^2)$ | $O(1)$ | Checks every unique pair $(i, j)$; slow for $n = 10^4$. |
| **Two-Pass Hash Table** | $O(n)$ | $O(n)$ | First pass builds map; second pass finds complements. |
| **One-Pass Hash Table (Optimal)** | $O(n)$ | $O(n)$ | Inserts and inspects simultaneously in a single traversal. |
