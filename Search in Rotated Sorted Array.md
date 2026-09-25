# 🎯 LeetCode 33: Search in Rotated Sorted Array

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly left rotated at an unknown index $k$ ($1 \le k < \text{nums.length}$) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be left rotated by 3 indices and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with $O(\log n)$ runtime complexity.

#### Examples

- **Example 1**:
  - **Input**: `nums = [4,5,6,7,0,1,2], target = 0`
  - **Output**: `4`
  - **Explanation**: Index $4$ holds the value $0$.

- **Example 2**:
  - **Input**: `nums = [4,5,6,7,0,1,2], target = 3`
  - **Output**: `-1`
  - **Explanation**: $3$ is not present in the array.

- **Example 3**:
  - **Input**: `nums = [1], target = 0`
  - **Output**: `-1`

#### Constraints

- $1 \le \text{nums.length} \le 5000$
- $-10^4 \le \text{nums}[i] \le 10^4$
- All values of `nums` are unique.
- `nums` is an ascending array that is possibly rotated.
- $-10^4 \le \text{target} \le 10^4$

---

### 💡 Intuition & Strategy

1. **Leveraging the Rotated Sorted Property**:
   - Even though the array is rotated, dividing it in half will **always result in at least one strictly sorted half**.
   - We can utilize a modified Binary Search to continuously narrow down our search space.

2. **Identifying the Sorted Half**:
   - Initialize two pointers: `left = 0` and `right = nums.length - 1`.
   - Calculate the middle index `mid`. If `nums[mid] == target`, return `mid`.
   - Determine which half is sorted:
     - If `nums[left] <= nums[mid]`, the **left half** is sorted.
     - Otherwise, the **right half** is sorted.

3. **Narrowing the Search Range**:
   - Check if the target falls within the boundaries of the sorted half.
   - If it does, restrict the search space to that half. Otherwise, search the other half.

4. **Complexity**:
   - **Time Complexity**: $O(\log N)$
     - The search space is halved with each iteration, matching standard binary search performance.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Only constant extra space is used for pointers.

---

### 💻 Java Solution

```java
/**
 * Problem: Search in Rotated Sorted Array (LeetCode 33)
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
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid;
            }
            
            // Check if the left half is sorted
            if (nums[left] <= nums[mid]) {
                // Check if target lies within the sorted left half
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } 
            // Otherwise, the right half must be sorted
            else {
                // Check if target lies within the sorted right half
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        
        return -1;
    }
}
