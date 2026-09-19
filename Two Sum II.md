# 🎯 LeetCode 167: Two Sum II - Input Array Is Sorted

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. 

Let these two numbers be `numbers[index1]` and `numbers[index2]` where $1 \le \text{index1} < \text{index2} \le \text{numbers.length}$.

Return the indices of the two numbers, `index1` and `index2`, added by one as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice. Your solution must use only constant extra space.

#### Examples

- **Example 1**:
  - **Input**: `numbers = [2,7,11,15]`, `target = 9`
  - **Output**: `[1,2]`
  - **Explanation**: The sum of 2 and 7 is 9. Therefore, $\text{index1} = 1$, $\text{index2} = 2$. We return `[1, 2]`.

- **Example 2**:
  - **Input**: `numbers = [2,3,4]`, `target = 6`
  - **Output**: `[1,3]`
  - **Explanation**: The sum of 2 and 4 is 6. Therefore $\text{index1} = 1$, $\text{index2} = 3$. We return `[1, 3]`.

- **Example 3**:
  - **Input**: `numbers = [-1,0]`, `target = -1`
  - **Output**: `[1,2]`
  - **Explanation**: The sum of -1 and 0 is -1. Therefore $\text{index1} = 1$, $\text{index2} = 2$. We return `[1, 2]`.

#### Constraints

- $2 \le \text{numbers.length} \le 3 \times 10^4$
- $-1000 \le \text{numbers}[i] \le 1000$
- `numbers` is sorted in non-decreasing order.
- $-1000 \le \text{target} \le 1000$
- The tests are generated such that there is exactly one solution.

---

### 💡 Intuition & Strategy

1. **Two-Pointer Technique**:
   - Since the array is already sorted in non-decreasing order, a brute-force search checking every pair would require $O(N^2)$ time. 
   - Instead, we can utilize a two-pointer approach, placing one pointer (`left`) at the beginning and the other (`right`) at the end of the array.

2. **Pointer Adjustment Based on Sum**:
   - Calculate the sum of elements at the current pointers: `sum = numbers[left] + numbers[right]`.
   - If `sum == target`, we have found our pair, so we return their 1-based indices (`[left + 1, right + 1]`).
   - If `sum < target`, the sum is too small, so we increment `left` to move to a larger value.
   - If `sum > target`, the sum is too large, so we decrement `right` to move to a smaller value.

3. **Complexity**:
   - **Time Complexity**: $O(N)$
     - In the worst-case scenario, the pointers traverse the array at most once.
   - **Space Complexity**: $O(1)$
     - Employs strictly auxiliary constant memory for tracking pointers, satisfying the problem constraints.

---

### 💻 Java Solution

```java
/**
 * Problem: Two Sum II - Input Array Is Sorted (LeetCode 167)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            
            if (sum == target) {
                // Return 1-based indices
                return new int[] { left + 1, right + 1 };
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        
        // Since the problem guarantees a unique solution, this point will never be reached.
        return new int[0];
    }
}
