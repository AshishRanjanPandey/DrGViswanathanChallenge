# 🎯 LeetCode 315: Count of Smaller Numbers After Self

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an integer array `nums`, return an integer array `counts` where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

#### Examples

- **Example 1**:
  - **Input**: `nums = [5,2,6,1]`
  - **Output**: `[2,1,1,0]`
  - **Explanation**: 
    - To the right of 5 there are 2 smaller elements (2 and 1).
    - To the right of 2 there is 1 smaller element (1).
    - To the right of 6 there is 1 smaller element (1).
    - To the right of 1 there is 0 smaller element.

- **Example 2**:
  - **Input**: `nums = [-1]`
  - **Output**: `[0]`

- **Example 3**:
  - **Input**: `nums = [-1,-1]`
  - **Output**: `[0,0]`

#### Constraints

- $1 \le \text{nums.length} \le 10^5$
- $-10^4 \le \text{nums}[i] \le 10^4$

---

### 💡 Intuition & Strategy

1. **Modified Merge Sort with Index Tracking**:
  - A brute-force $O(N^2)$ approach checks all elements to the right for each element, leading to Time Limit Exceeded (TLE) since $N = 10^5$.
  - Divide and conquer via **Merge Sort** allows us to count inversions efficiently in $O(N \log N)$ time.
  - To maintain track of original positions during sorting, we sort an array of `indices` rather than mutating `nums` directly.

2. **Counting Inversions During Merging**:
  - In Merge Sort, when merging two sorted halves (`left` and `right`), all elements in `right` were originally to the right of all elements in `left`.
  - We maintain a counter `rightCount` for elements taken from the `right` subarray that are strictly smaller than the current element from the `left` subarray.
  - When a `left` element `nums[indices[i]]` is selected to be placed into `temp`, we increment its total count by `rightCount`.

3. **Algorithm Execution**:
  - Recursively split `indices` into halves until single-element segments remain.
  - Merge left and right halves while updating `counts` for elements coming from the left branch whenever smaller elements from the right branch pass them.

4. **Complexity**:
  - **Time Complexity**: $O(N \log N)$
    - Splitting takes $O(\log N)$ levels, and merging takes $O(N)$ work per level.
  - **Space Complexity**: $O(N)$ auxiliary memory
    - Requires auxiliary arrays for indices and temporary merging (`temp`), plus $O(\log N)$ recursion stack depth.

---

### 💻 Java Solution

```java
/**
- Problem: Count of Smaller Numbers After Self (LeetCode 315)
- Language: Java 17
- Time Complexity: O(N log N)
- Space Complexity: O(N) auxiliary
- Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> countSmaller(int[] nums) {
        int n = nums.length;
        int[] counts = new int[n];
        int[] indices = new int[n];
        int[] temp = new int[n];

        for (int i = 0; i < n; i++) {
            indices[i] = i;
        }

        mergeSort(nums, indices, temp, counts, 0, n - 1);

        List<Integer> result = new ArrayList<>(n);
        for (int count : counts) {
            result.add(count);
        }
        return result;
    }

    private void mergeSort(int[] nums, int[] indices, int[] temp, int[] counts, int left, int right) {
        if (left >= right) {
            return;
        }

        int mid = left + (right - left) / 2;
        mergeSort(nums, indices, temp, counts, left, mid);
        mergeSort(nums, indices, temp, counts, mid + 1, right);
        merge(nums, indices, temp, counts, left, mid, right);
    }

    private void merge(int[] nums, int[] indices, int[] temp, int[] counts, int left, int mid, int right) {
        int i = left;       // Pointer for left half
        int j = mid + 1;    // Pointer for right half
        int k = left;       // Pointer for merged temp array
        int rightCount = 0; // Count of elements from right half smaller than current left element

        while (i <= mid && j <= right) {
            if (nums[indices[j]] < nums[indices[i]]) {
                temp[k++] = indices[j++];
                rightCount++;
            } else {
                counts[indices[i]] += rightCount;
                temp[k++] = indices[i++];
            }
        }

        while (i <= mid) {
            counts[indices[i]] += rightCount;
            temp[k++] = indices[i++];
        }

        while (j <= right) {
            temp[k++] = indices[j++];
        }

        for (int p = left; p <= right; p++) {
            indices[p] = temp[p];
        }
    }
}
```
