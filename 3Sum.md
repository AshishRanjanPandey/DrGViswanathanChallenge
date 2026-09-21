# 🎯 LeetCode 15: 3Sum

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

#### Examples

- **Example 1**:
  - **Input**: `nums = [-1,0,1,2,-1,-4]`
  - **Output**: `[[-1,-1,2],[-1,0,1]]`
  - **Explanation**: 
    - `nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0`
    - `nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0`
    - `nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0`
    - Distinct triplets are `[-1,0,1]` and `[-1,-1,2]`.

- **Example 2**:
  - **Input**: `nums = [0,1,1]`
  - **Output**: `[]`
  - **Explanation**: The only possible triplet does not sum up to $0$.

- **Example 3**:
  - **Input**: `nums = [0,0,0]`
  - **Output**: `[[0,0,0]]`
  - **Explanation**: The only possible triplet sums up to $0$.

#### Constraints

- $3 \le \text{nums.length} \le 3000$
- $-10^5 \le \text{nums}[i] \le 10^5$

---

### 💡 Intuition & Strategy

1. **Sorting the Array**:
   - Sorting in ascending order transforms an unsorted search space into an ordered domain.
   - It allows using the **Two-Pointer technique** to locate pairs in $O(N)$ time per iteration and groups identical numbers together to simplify deduplication.

2. **Fixing the Anchor & Two-Pointer Sweep**:
   - Iterate index `i` from `0` to `nums.length - 3` as the first element of the triplet.
   - For each fixed `nums[i]`, initialize two pointers: `l = i + 1` (left) and `r = nums.length - 1` (right).
   - Evaluate `sum = nums[i] + nums[l] + nums[r]`:
     - If `sum == 0`: Record the triplet `[nums[i], nums[l], nums[r]]`, then shift `l` forward and `r` backward.
     - If `sum < 0`: The sum is too small; move `l` rightward to increase the value.
     - If `sum > 0`: The sum is too large; move `r` leftward to decrease the value.

3. **Handling Duplicate Triplets Without Extra Space**:
   - **Outer Anchor Dedup**: If `i > 0` and `nums[i] == nums[i - 1]`, `continue` to avoid repeating triplets already formed by identical values.
   - **Inner Pointer Dedup**: When a match (`sum == 0`) is found, advance `l` while `nums[l] == nums[l + 1]` and retreat `r` while `nums[r] == nums[r - 1]` before shifting to unseen candidate values.

4. **Complexity**:
   - **Time Complexity**: $O(N^2)$
     - Sorting takes $O(N \log N)$.
     - The outer loop runs $N - 2$ times, and the two-pointer sweep inside takes $O(N)$, resulting in $O(N^2)$ total operations.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Beyond output storage, only standard sorting overhead ($O(\log N)$ or $O(N)$ depending on the internal dual-pivot Quicksort/Timsort implementation) is required.

---

### 💻 Java Solution

```java
/**
 * Problem: 3Sum (LeetCode 15)
 * Language: Java 17
 * Time Complexity: O(N^2)
 * Space Complexity: O(1) auxiliary (excluding output & sort stack)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {
            // Early break: remaining elements cannot sum to zero if the minimum is positive
            if (nums[i] > 0) {
                break;
            }

            // Skip duplicate fixed elements
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int l = i + 1;
            int r = nums.length - 1;

            while (l < r) {
                int sum = nums[i] + nums[l] + nums[r];

                if (sum == 0) {
                    result.add(Arrays.asList(nums[i], nums[l], nums[r]));

                    // Skip duplicates for left and right pointers
                    while (l < r && nums[l] == nums[l + 1]) {
                        l++;
                    }
                    while (l < r && nums[r] == nums[r - 1]) {
                        r--;
                    }

                    l++;
                    r--;
                } else if (sum < 0) {
                    l++;
                } else {
                    r--;
                }
            }
        }

        return result;
    }
}
