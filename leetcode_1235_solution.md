# 🎯 LeetCode 1235: Maximum Profit in Job Scheduling

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

We have $n$ jobs, where every job is scheduled to be done from `startTime[i]` to `endTime[i]`, obtaining a profit of `profit[i]`.

You're given the `startTime`, `endTime`, and `profit` arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.

If you choose a job that ends at time $X$ you will be able to start another job that starts at time $X$.

#### Examples

- **Example 1**:
  - **Input**: `startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]`
  - **Output**: `120`
  - **Explanation**: 
    - The subset chosen is the first and fourth job. 
    - Time range $[1-3] + [3-6]$, profit obtained $= 120 = 50 + 70$.

- **Example 2**:
  - **Input**: `startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]`
  - **Output**: `150`
  - **Explanation**: 
    - The subset chosen is the first, fourth, and fifth job. 
    - Profit obtained $= 150 = 20 + 70 + 60$.

- **Example 3**:
  - **Input**: `startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]`
  - **Output**: `6`
  - **Explanation**: 
    - The subset chosen is the second job with time range $[1-3]$ and profit $6$.

#### Constraints

- $1 \le \text{startTime.length} == \text{endTime.length} == \text{profit.length} \le 5 \times 10^4$
- $1 \le \text{startTime}[i] < \text{endTime}[i] \le 10^9$
- $1 \le \text{profit}[i] \le 10^4$

---

### 💡 Intuition & Strategy

1. **Job Structuring & Sorting by End Time**:
  - Combine `startTime[i]`, `endTime[i]`, and `profit[i]` into a 2D array representation.
  - Sort all jobs in ascending order based on their **`endTime`**. Sorting by end time ensures that when evaluating job $i$, all candidate non-overlapping predecessor jobs have already been processed.

2. **Dynamic Programming State Definition**:
  - Define `dp[i]` as the maximum profit achievable considering a subset of the first $i$ jobs ($1$-indexed).
  - For the $i$-th job ($0$-indexed in the sorted array), we make a choice:
    - **Skip Job $i$**: The maximum profit remains `dp[i]`.
    - **Take Job $i$**: The profit will be `jobs[i].profit` plus the maximum profit achievable from jobs that end on or before `jobs[i].startTime`.

3. **Binary Search for Last Non-Overlapping Job**:
  - To locate the latest non-overlapping job in $O(\log N)$ time, use **Binary Search** (`upperBound`) on the sorted jobs array.
  - Search for the count of jobs with $\text{endTime} \le \text{jobs}[i].\text{startTime}$, which directly maps to the DP state index of the last valid job.

4. **Complexity**:
  - **Time Complexity**: $O(N \log N)$
    - Sorting $N$ jobs takes $O(N \log N)$ time.
    - The DP array traversal runs $N$ times, with each step executing a binary search in $O(\log N)$ time, resulting in an overall time complexity of $O(N \log N)$.
  - **Space Complexity**: $O(N)$
    - Requires $O(N)$ auxiliary space to store the bundled `jobs` array and the `dp` table.

---

### 💻 Java Solution

```java
/**
 * Problem: Maximum Profit in Job Scheduling (LeetCode 1235)
 * Language: Java 17
 * Time Complexity: O(N log N)
 * Space Complexity: O(N) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.Arrays;

class Solution {
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        int n = startTime.length;
        int[][] jobs = new int[n][3];

        for (int i = 0; i < n; i++) {
            jobs[i] = new int[]{startTime[i], endTime[i], profit[i]};
        }

        // Sort jobs by endTime ascending
        Arrays.sort(jobs, (a, b) -> Integer.compare(a[1], b[1]));

        // dp[i] represents max profit using a subset of the first i jobs
        int[] dp = new int[n + 1];

        for (int i = 0; i < n; i++) {
            int start = jobs[i][0];
            int currentProfit = jobs[i][2];

            // Binary search to find count of jobs ending <= current job's startTime
            int prevJobIndex = upperBound(jobs, i, start);

            // Max profit skipping vs. taking the current job
            dp[i + 1] = Math.max(dp[i], dp[prevJobIndex] + currentProfit);
        }

        return dp[n];
    }

    // Returns index of the first job with endTime > targetStart
    // (matches the number of jobs with endTime <= targetStart)
    private int upperBound(int[][] jobs, int high, int targetStart) {
        int low = 0;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (jobs[mid][1] <= targetStart) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}
```