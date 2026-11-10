# 🎯 LeetCode 4: Median of Two Sorted Arrays

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given two sorted arrays `nums1` and `nums2` of size $m$ and $n$ respectively, return the median of the two sorted arrays.

The overall run time complexity should be $O(\log (m+n))$.

#### Examples

- **Example 1**:
  - **Input**: `nums1 = [1,3]`, `nums2 = [2]`
  - **Output**: `2.00000`
  - **Explanation**: Merged array = `[1,2,3]` and median is $2$.

- **Example 2**:
  - **Input**: `nums1 = [1,2]`, `nums2 = [3,4]`
  - **Output**: `2.50000`
  - **Explanation**: Merged array = `[1,2,3,4]` and median is $(2 + 3) / 2 = 2.5$.

#### Constraints

- $\text{nums1.length} == m$
- $\text{nums2.length} == n$
- $0 \le m \le 1000$
- $0 \le n \le 1000$
- $1 \le m + n \le 2000$
- $-10^6 \le \text{nums1}[i], \text{nums2}[i] \le 10^6$

---

### 💡 Intuition & Strategy

1. **Binary Search on the Smaller Array**:
   - Merging two arrays takes $O(m + n)$ time, which does not meet the $O(\log(m+n))$ requirement.
   - Instead, we perform a **Binary Search** on the partition of the smaller array (`nums1`) so that search time is bounded by $O(\log(\min(m, n)))$.

2. **Partitioning the Search Space**:
   - We split both arrays into left and right halves such that the total number of elements in the combined left half is equal to $\lfloor (m + n + 1) / 2 \rfloor$.
   - Let index $i$ partition `nums1` and index $j = (m + n + 1) / 2 - i$ partition `nums2`.

3. **Evaluating Boundary Conditions**:
   - Identify boundary values around partitions:
     - $\text{leftA} = \text{nums1}[i - 1]$ (or $-\infty$ if $i = 0$)
     - $\text{rightA} = \text{nums1}[i]$ (or $\infty$ if $i = m$)
     - $\text{leftB} = \text{nums2}[j - 1]$ (or $-\infty$ if $j = 0$)
     - $\text{rightB} = \text{nums2}[j]$ (or $\infty$ if $j = n$)
   - A valid partition occurs when $\text{leftA} \le \text{rightB}$ and $\text{leftB} \le \text{rightA}$.

4. **Calculating the Median**:
   - **Odd Total Length**: The median is $\max(\text{leftA}, \text{leftB})$.
   - **Even Total Length**: The median is $\frac{\max(\text{leftA}, \text{leftB}) + \min(\text{rightA}, \text{rightB})}{2.0}$.

5. **Complexity**:
   - **Time Complexity**: $O(\log(\min(m, n)))$ — Binary search is performed on the smaller array.
   - **Space Complexity**: $O(1)$ auxiliary memory — Search is conducted in-place without extra allocations.

---

### 💻 Java Solution

```java
/**
 * Problem: Median of Two Sorted Arrays (LeetCode 4)
 * Language: Java 17
 * Time Complexity: O(log(min(m, n)))
 * Space Complexity: O(1) auxiliary memory
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        // Ensure nums1 is the smaller array to guarantee O(log(min(m, n)))
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int m = nums1.length;
        int n = nums2.length;

        int low = 0, high = m;

        while (low <= high) {
            int i = (low + high) / 2;
            int j = (m + n + 1) / 2 - i;

            int leftA  = (i == 0) ? Integer.MIN_VALUE : nums1[i - 1];
            int rightA = (i == m) ? Integer.MAX_VALUE : nums1[i];

            int leftB  = (j == 0) ? Integer.MIN_VALUE : nums2[j - 1];
            int rightB = (j == n) ? Integer.MAX_VALUE : nums2[j];

            // Correct partition found
            if (leftA <= rightB && leftB <= rightA) {

                // Even total length
                if ((m + n) % 2 == 0) {
                    return (Math.max(leftA, leftB) + 
                            Math.min(rightA, rightB)) / 2.0;
                }
                // Odd total length
                else {
                    return Math.max(leftA, leftB);
                }
            }
            // Move left in nums1
            else if (leftA > rightB) {
                high = i - 1;
            }
            // Move right in nums1
            else {
                low = i + 1;
            }
        }
        return 0.0; // Unreachable for valid sorted input arrays
    }
}
```