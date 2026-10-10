# 🎯 LeetCode 215: Kth Largest Element in an Array

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an integer array `nums` and an integer `k`, return the $k$th largest element in the array.

Note that it is the $k$th largest element in the sorted order, not the $k$th distinct element.

You must solve it without sorting.

#### Examples

- **Example 1**:
  - **Input**: `nums = [3,2,1,5,6,4]`, `k = 2`
  - **Output**: `5`

- **Example 2**:
  - **Input**: `nums = [3,2,3,1,2,4,5,5,6]`, `k = 4`
  - **Output**: `4`

#### Constraints

- $1 \le k \le \text{nums.length} \le 10^5$
- $-10^4 \le \text{nums}[i] \le 10^4$

---

### 💡 Intuition & Strategy

1. **Quickselect Algorithm with 3-Way Partitioning**:
   - Standard Quickselect finds the $k$th element in $O(N)$ average time without sorting the entire array. However, standard 2-way partitioning degenerates to $O(N^2)$ time when the array contains heavy duplicate elements.
   - Using **3-way partitioning (Dutch National Flag algorithm)** groups all elements equal to the pivot together in the center, completely avoiding redundant work and handling duplicate-heavy test cases efficiently.

2. **Randomized Pivot Selection**:
   - To prevent worst-case performance on maliciously crafted or sorted inputs, a random pivot index is chosen within the current search bounds.

3. **Target Index Mapping**:
   - The $k$th largest element corresponds to the index `nums.length - k` if the array were sorted in ascending order. We search directly for this target index.

4. **Complexity**:
   - **Time Complexity**: $O(N)$ average case, $O(N^2)$ worst-case (mitigated by randomization).
   - **Space Complexity**: $O(1)$ auxiliary space (excluding the recursion stack depth of $O(\log N)$ average).

---

### 💻 Java Solution

```java
/**
 * Problem: Kth Largest Element in an Array (LeetCode 215)
 * Language: Java 17
 * Time Complexity: O(N) average
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.Random;

class Solution {
    private static final Random RANDOM = new Random();

    public int findKthLargest(int[] nums, int k) {
        // Convert "kth largest" to 0-based index in ascending sorted order
        int kTarget = nums.length - k;
        return quickSelect(nums, 0, nums.length - 1, kTarget);
    }

    private int quickSelect(int[] nums, int left, int right, int kTarget) {
        if (left == right) {
            return nums[left];
        }

        // Randomize pivot to avoid worst-case performance
        int pivotIndex = left + RANDOM.nextInt(right - left + 1);
        int pivotValue = nums[pivotIndex];

        // 3-way partitioning:
        // nums[left ... lt-1] < pivotValue
        // nums[lt ... gt] == pivotValue
        // nums[gt+1 ... right] > pivotValue
        int lt = left, i = left, gt = right;
        swap(nums, pivotIndex, right); // Move pivot to end temporarily

        while (i <= gt) {
            if (nums[i] < pivotValue) {
                swap(nums, lt++, i++);
            } else if (nums[i] > pivotValue) {
                swap(nums, i, gt--);
            } else {
                i++;
            }
        }

        // Check if kTarget falls inside the duplicate pivot range
        if (kTarget >= lt && kTarget <= gt) {
            return nums[kTarget];
        } else if (kTarget < lt) {
            return quickSelect(nums, left, lt - 1, kTarget);
        } else {
            return quickSelect(nums, gt + 1, right, kTarget);
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
