# 🎯 LeetCode 704: Binary Search

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with $O(\log n)$ runtime complexity.

#### Examples

- **Example 1**:
  - **Input**: `nums = [-1,0,3,5,9,12], target = 9`
  - **Output**: `4`
  - **Explanation**: `9` exists in `nums` and its index is `4`.

- **Example 2**:
  - **Input**: `nums = [-1,0,3,5,9,12], target = 2`
  - **Output**: `-1`
  - **Explanation**: `2` does not exist in `nums`, so return `-1`.

#### Constraints

- $1 \le \text{nums.length} \le 10^4$
- $-10^4 < \text{nums}[i], \text{target} < 10^4$
- All the integers in `nums` are unique.
- `nums` is sorted in ascending order.

---

### 💡 Intuition & Strategy

1. **Leveraging Sorted Property via Binary Search**:
   - Since the array is already sorted in ascending order, we don't need to scan it linearly in $O(N)$ time.
   - Instead, we can divide the search space in half at each step, achieving the required $O(\log n)$ runtime complexity.

2. **Pointer Management (`left` and `right`)**:
   - Initialize two pointers: `left = 0` pointing to the start of the array, and `right = nums.length - 1` pointing to the end.
   - Compute the middle index: `mid = left + (right - left) / 2` (using this formulation prevents potential integer overflow for extremely large arrays).

3. **Comparison & Shrinking Search Space**:
   - Compare `nums[mid]` with the `target`:
     - If `nums[mid] == target`: The target is found; return `mid`.
     - If `nums[mid] < target`: The target must lie in the right half. Update `left = mid + 1`.
     - If `nums[mid] > target`: The target must lie in the left half. Update `right = mid - 1`.
   - Repeat the loop until `left > right`. If the loop terminates without finding the target, return `-1`.

4. **Complexity**:
   - **Time Complexity**: $O(\log n)$ — The search space is halved with every iteration.
   - **Space Complexity**: $O(1)$ auxiliary memory — Only a few primitive pointer variables are used.

---

### 💻 Java Solution

```java
/**
 * Problem: Binary Search (LeetCode 704)
 * Language: Java 17
 * Time Complexity: O(log N)
 * Space Complexity: O(1)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left <= right) {
            // Prevent potential integer overflow for large index ranges
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid; // Target found at index mid
            } else if (nums[mid] < target) {
                left = mid + 1; // Target is in the right half
            } else {
                right = mid - 1; // Target is in the left half
            }
        }
        
        return -1; // Target does not exist in nums
    }
}
