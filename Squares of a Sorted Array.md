# 🎯 LeetCode 977: Squares of a Sorted Array

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an integer array `nums` sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

#### Examples

- **Example 1**:
  - **Input**: `nums = [-4,-1,0,3,10]`
  - **Output**: `[0,1,9,16,100]`
  - **Explanation**: 
    - After squaring, the array becomes `[16,1,0,9,100]`.
    - After sorting, it becomes `[0,1,9,16,100]`.

- **Example 2**:
  - **Input**: `nums = [-7,-3,2,3,11]`
  - **Output**: `[4,9,9,49,121]`

#### Constraints

- $1 \le \text{nums.length} \le 10^4$
- $-10^4 \le \text{nums}[i] \le 10^4$
- `nums` is sorted in non-decreasing order.

---

### 💡 Intuition & Strategy

1. **Leveraging Sorted Properties with Two Pointers**:
   - Since the original array is sorted, the numbers with the largest magnitude (and thus the largest squares) will always lie at the extreme ends of the array—either the large negative numbers on the far left or the large positive numbers on the far right.
   - We can place one pointer (`left`) at the beginning and another pointer (`right`) at the end of the array.

2. **Filling the Result Array Back-to-Front**:
   - Initialize a result array of the same length and a tracking pointer `pos` starting at the very end of the result array (`nums.length - 1`).
   - Compare the squared values of `nums[left]` and `nums[right]`.
   - Place the larger square into `result[pos]`, and move the respective pointer inward (`left++` or `right--`). Then decrement `pos`. This guarantees that the result array is populated in descending order of squares, naturally sorting it in non-decreasing order from front to back.

3. **Complexity**:
   - **Time Complexity**: $O(N)$
     - We traverse the array a single time using two converging pointers, doing constant time operations per step.
   - **Space Complexity**: $O(N)$ 
     - Required to store and return the output array (auxiliary space beyond the result array is $O(1)$).

---

### 💻 Java Solution

```java
/**
 * Problem: Squares of a Sorted Array (LeetCode 977)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(N) for output array (O(1) auxiliary)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        int left = 0;
        int right = n - 1;
        int pos = n - 1; // Fill the result array from back to front
        
        while (left <= right) {
            int leftSquare = nums[left] * nums[left];
            int rightSquare = nums[right] * nums[right];
            
            if (leftSquare > rightSquare) {
                result[pos] = leftSquare;
                left++;
            } else {
                result[pos] = rightSquare;
                right--;
            }
            pos--;
        }
        
        return result;
    }
}
