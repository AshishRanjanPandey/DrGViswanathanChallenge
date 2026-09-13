# 🎯 LeetCode 56: Merge Intervals

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

#### Examples

- **Example 1**:
  - **Input**: `intervals = [[1,3],[2,6],[8,10],[15,18]]`
  - **Output**: `[[1,6],[8,10],[15,18]]`
  - **Explanation**: Since intervals `[1,3]` and `[2,6]` overlap, merge them into `[1,6]`.

- **Example 2**:
  - **Input**: `intervals = [[1,4],[4,5]]`
  - **Output**: `[[1,5]]`
  - **Explanation**: Intervals `[1,4]` and `[4,5]` are considered overlapping.

- **Example 3**:
  - **Input**: `intervals = [[4,7],[1,4]]`
  - **Output**: `[[1,7]]`
  - **Explanation**: Intervals `[1,4]` and `[4,7]` are considered overlapping.

#### Constraints

- 1 <= intervals.length <= 10^4
- intervals[i].length == 2
- 0 <= starti <= endi <= 10^4

---

### 💡 Intuition & Strategy

1. **Sorting by Start Time**:
   - Arbitrarily ordered intervals require an exhaustive comparison of every pair to identify merges, which is inefficient.
   - By sorting intervals based on their start coordinates (`intervals[i][0]`), overlapping intervals are guaranteed to appear adjacently in the sorted sequence.

2. **Sequential Merging Logic**:
   - Maintain a dynamically sized list `merged` and seed it with the first interval as `current`.
   - Iterate through subsequent intervals `next` starting from index `1`:
     - **Overlap Condition**: If `next[0] <= current[1]`, the intervals overlap. Expand the right boundary:
       `current[1] = Math.max(current[1], next[1])`
     - **Disjoint Condition**: If `next[0] > current[1]`, there is no overlap. Reassign `current = next` and add it to `merged`.

3. **In-Place Reference Modification**:
   - Because objects in Java are handled via reference, modifying `current[1]` in place directly updates the interval stored inside the `merged` collection, avoiding extra array copy operations.

4. **Complexity**:
   - **Time Complexity**: O(N log N)
     - Sorting the N intervals takes O(N log N) time.
     - The subsequent linear scan processes each interval once in O(N) time.
     - Overall time is dominated by the sort: O(N log N).
   - **Space Complexity**: O(N)
     - The output list `merged` holds up to N non-overlapping intervals in the worst case.
     - The sorting algorithm consumes O(log N) auxiliary space for call stack frames.

---

### 💻 Java Solution

```java
/**
 * Problem: Merge Intervals (LeetCode 56)
 * Language: Java 17
 * Time Complexity: O(N log N)
 * Space Complexity: O(N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {

    public int[][] merge(int[][] intervals) {
        if (intervals.length <= 1) {
            return intervals;
        }

        // Step 1: Sort intervals ascending by start time
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> merged = new ArrayList<>();

        // Step 2: Initialize with the first interval
        int[] current = intervals[0];
        merged.add(current);

        // Step 3: Traverse remaining intervals and merge if overlapping
        for (int i = 1; i < intervals.length; i++) {
            int[] next = intervals[i];

            if (next[0] <= current[1]) {
                // Overlap: expand end point of current interval
                current[1] = Math.max(current[1], next[1]);
            } else {
                // Disjoint: advance pointer and track next interval
                current = next;
                merged.add(current);
            }
        }

        return merged.toArray(new int[0][]);
    }
}
