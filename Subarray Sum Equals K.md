# 🎯 LeetCode 560: Subarray Sum Equals K

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an array of integers `nums` and an integer `k`, return the total number of subarrays whose sum equals to `k`.

A subarray is a contiguous non-empty sequence of elements within an array.

#### Examples

- **Example 1**:
  - **Input**: `nums = [1,1,1]`, `k = 2`
  - **Output**: `2`
  - **Explanation**: Subarrays with a sum of 2 are `[1, 1]` (from index 0 to 1) and `[1, 1]` (from index 1 to 2).

- **Example 2**:
  - **Input**: `nums = [1,2,3]`, `k = 3`
  - **Output**: `2`
  - **Explanation**: Subarrays with a sum of 3 are `[1, 2]` (from index 0 to 1) and `[3]` (from index 2 to 2).

#### Constraints

- $1 \le \text{nums.length} \le 2 \times 10^4$
- $-1000 \le \text{nums}[i] \le 1000$
- $-10^7 \le k \le 10^7$

---

### 💡 Intuition & Strategy

1. **Prefix Sum Concept**:
   - Instead of checking every possible starting and ending index with a brute-force approach (which would take $O(N^2)$ or $O(N^3)$ time), we can compute a running **Prefix Sum**.
   - Let `currentSum` be the cumulative sum of elements from the beginning of the array up to the current index.

2. **Using a Hash Map for Lookup**:
   - If the prefix sum up to the current index is `currentSum`, and there exists a previous prefix sum equal to `currentSum - k`, then the subarray(s) formed between that previous point and the current index must sum up to `k`.
   - We utilize a `HashMap` to store the frequencies of every prefix sum encountered so far. This allows us to find and accumulate matching previous prefix sums in $O(1)$ time.

3. **Base Case Handling**:
   - We insert `(0, 1)` into the hash map initially. This handles scenarios where a prefix sum itself equals `k` right from the beginning of the array (subarrays starting from index 0).

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - We traverse the array of length $N$ exactly once. Hash map insertions and lookups take $O(1)$ average time.
   - **Space Complexity**: $O(N)$
     - In the worst case, all prefix sums are unique, requiring space in the hash map proportional to the number of elements.

---

### 💻 Java Solution

```java
/**
 * Problem: Subarray Sum Equals K (LeetCode 560)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int currentSum = 0;
        
        // Map to store (prefixSum, frequency)
        Map<Integer, Integer> prefixSumMap = new HashMap<>();
        
        // Base case: A prefix sum of 0 has occurred once (to handle subarrays starting from index 0)
        prefixSumMap.put(0, 1);
        
        for (int num : nums) {
            currentSum += num;
            
            // Check if there is a previous prefix sum that we can subtract from currentSum to get k
            int target = currentSum - k;
            if (prefixSumMap.containsKey(target)) {
                count += prefixSumMap.get(target);
            }
            
            // Add the currentSum to the map, updating its frequency
            prefixSumMap.put(currentSum, prefixSumMap.getOrDefault(currentSum, 0) + 1);
        }
        
        return count;
    }
}
```
