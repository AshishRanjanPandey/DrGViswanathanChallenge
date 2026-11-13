# 🎯 LeetCode 300: Longest Increasing Subsequence

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

#### Examples

- **Example 1**:
  - **Input**: `nums = [10,9,2,5,3,7,101,18]`
  - **Output**: `4`
  - **Explanation**: The longest increasing subsequence is `[2,3,7,101]`, therefore the length is $4$.

- **Example 2**:
  - **Input**: `nums = [0,1,0,3,2,3]`
  - **Output**: `4`

- **Example 3**:
  - **Input**: `nums = [7,7,7,7,7,7,7]`
  - **Output**: `1`

#### Constraints

- $1 \le \text{nums.length} \le 2500$
- $-10^4 \le \text{nums}[i] \le 10^4$

**Follow up**: Can you come up with an algorithm that runs in $O(N \log N)$ time complexity?

---

### 💡 Intuition & Strategy

To achieve the optimal $O(N \log N)$ time complexity, we use a combination of **Dynamic Programming** and **Binary Search** (often referred to as Patience Sorting).

1. **The `tails` Array**:
   - We maintain an array called `tails` where `tails[i]` stores the smallest ending element (tail) of all valid increasing subsequences of length `i + 1`. 
   - Keeping the tails as small as possible is crucial because it leaves the most room for future elements to extend the sequence.

2. **Binary Search Sweep**:
   - Because we only append larger elements, the `tails` array is naturally sorted in strictly increasing order. This allows us to use Binary Search.
   - We iterate through each number `x` in the input array `nums`.
   - For each `x`, we binary search the `tails` array to find the first element that is greater than or equal to `x`.

3. **Updating the State**:
   - **Overwrite**: If we find an element in `tails` $\ge x$, we replace it with `x`. This doesn't increase the length of our longest subsequence, but it lowers the barrier for subsequent numbers to be appended.
   - **Extend**: If `x` is greater than all elements currently in our active `tails` array, it means `x` can extend our longest found subsequence. We place it at the end and increment our total `size`.

4. **Complexity**:
   - **Time Complexity**: $O(N \log N)$
     - We iterate through the `nums` array of length $N$ exactly once.
     - For each element, the binary search operation takes $O(\log N)$ time.
   - **Space Complexity**: $O(N)$
     - We require an auxiliary `tails` array, which in the worst-case scenario (a strictly increasing input array) will store $N$ elements.

---

### 💻 Java Solution

```java
/**
 * Problem: Longest Increasing Subsequence (LeetCode 300)
 * Language: Java 17
 * Time Complexity: O(N log N)
 * Space Complexity: O(N) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int lengthOfLIS(int[] nums) {
        // tails[i] stores the smallest tail of all increasing subsequences of length i+1
        int[] tails = new int[nums.length];
        int size = 0;
        
        for (int x : nums) {
            int left = 0, right = size;
            
            // Binary search to find the index where 'x' can replace an existing element
            // or extend the longest subsequence found so far
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (tails[mid] < x) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            
            // Overwrite the current tail or append x if it's the largest
            tails[left] = x;
            
            // If x was placed at the end of our current logical array, increase size
            if (left == size) {
                size++;
            }
        }
        
        return size;
    }
}
```
