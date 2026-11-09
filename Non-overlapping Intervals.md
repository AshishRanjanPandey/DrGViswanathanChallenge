# 🎯 LeetCode 435: Non-overlapping Intervals

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an array of intervals `intervals` where `intervals[i] = [start_i, end_i]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

Note that intervals which only touch at a point are non-overlapping. For example, `[1, 2]` and `[2, 3]` are non-overlapping.

#### Examples

- **Example 1**:
  - **Input**: `intervals = [[1,2],[2,3],[3,4],[1,3]]`
  - **Output**: `1`
  - **Explanation**: `[1,3]` can be removed and the rest of the intervals are non-overlapping.

- **Example 2**:
  - **Input**: `intervals = [[1,2],[1,2],[1,2]]`
  - **Output**: `2`
  - **Explanation**: You need to remove two `[1,2]` to make the rest of the intervals non-overlapping.

- **Example 3**:
  - **Input**: `intervals = [[1,2],[2,3]]`
  - **Output**: `0`
  - **Explanation**: You don't need to remove any of the intervals since they're already non-overlapping.

#### Constraints

- $1 \le \text{intervals.length} \le 10^5$
- $\text{intervals}[i]\text{.length} == 2$
- $-5 \times 10^4 \le \text{start}_i < \text{end}_i \le 5 \times 10^4$

---

### 💡 Intuition & Strategy

1. **Sorting by End Time (Greedy Approach)**:
   - To minimize the number of intervals you remove, you must maximize the number of non-overlapping intervals you keep.
   - By sorting the intervals based on their end times in ascending order, you can safely pick the interval that ends the earliest. This leaves the maximum possible room for subsequent intervals to fit in without overlapping.

2. **Tracking the End Bound**:
   - Keep track of a `currentEnd` variable, initialized to the end time of the first interval (after sorting).
   - Initialize a `removed` counter to $0$.

3. **Iterate and Compare**:
   - Loop through the rest of the intervals starting from index 1.
   - For each interval, check if its start time is strictly less than `currentEnd`:
     - **If it is less (Overlap)**: It conflicts with the previously chosen interval. Because the current interval ends *after or at the same time* as the previous one (due to sorting), keeping it would be suboptimal. We increment the `removed` counter.
     - **If it is greater or equal (No Overlap)**: We can safely keep this interval. We update `currentEnd` to this new interval's end time.

4. **Complexity**:
   - **Time Complexity**: $O(N \log N)$
     - Sorting the `intervals` array dominates the time complexity, taking $O(N \log N)$ time.
     - The subsequent linear scan through the array takes $O(N)$ time, resulting in a total time complexity of $O(N \log N)$.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Beyond the space required by Java's internal sorting algorithm ($O(\log N)$ space for quicksort variations), no additional data structures are used.

---

### 💻 Java Solution

```java
/**
 * Problem: Non-overlapping Intervals (LeetCode 435)
 * Language: Java 17
 * Time Complexity: O(N log N)
 * Space Complexity: O(1) auxiliary (excluding sort stack)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.Arrays;

class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals == null || intervals.length == 0) {
            return 0;
        }
        
        // Sort the intervals by their end times in ascending order
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));
        
        int removed = 0;
        int currentEnd = intervals[0][1];
        
        // Iterate through the sorted intervals starting from the second one
        for (int i = 1; i < intervals.length; i++) {
            // If the start time of the current interval is strictly less than 
            // the end time of the previous interval, they overlap.
            if (intervals[i][0] < currentEnd) {
                removed++; // We must remove the current one to minimize overlaps
            } else {
                // No overlap, update the end time to the current interval's end time
                currentEnd = intervals[i][1];
            }
        }
        
        return removed;
    }
}
```
