# 🎯 LeetCode 621: Task Scheduler

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an array of CPU tasks, each labeled with a letter from A to Z, and a number $n$. Each CPU interval can be idle or allow the completion of one task. Tasks can be completed in any order, but there's a constraint: there has to be a gap of at least $n$ intervals between two tasks with the same label.

Return the minimum number of CPU intervals required to complete all tasks.

#### Examples

- **Example 1**:
  - **Input**: `tasks = ["A","A","A","B","B","B"]`, `n = 2`
  - **Output**: `8`
  - **Explanation**: 
    - A possible sequence is: `A -> B -> idle -> A -> B -> idle -> A -> B`.
    - After completing task A, you must wait two intervals before doing A again. The same applies to task B. In the 3rd interval, neither A nor B can be done, so you idle. By the 4th interval, you can do A again as 2 intervals have passed.

- **Example 2**:
  - **Input**: `tasks = ["A","C","A","B","D","B"]`, `n = 1`
  - **Output**: `6`
  - **Explanation**: A possible sequence is: `A -> B -> C -> D -> A -> B`. With a cooling interval of 1, you can repeat a task after just one other task.

- **Example 3**:
  - **Input**: `tasks = ["A","A","A", "B","B","B"]`, `n = 3`
  - **Output**: `10`
  - **Explanation**: A possible sequence is: `A -> B -> idle -> idle -> A -> B -> idle -> idle -> A -> B`. There are only two types of tasks, A and B, which need to be separated by 3 intervals. This leads to idling twice between repetitions of these tasks.

#### Constraints

- $1 \le \text{tasks.length} \le 10^4$
- `tasks[i]` is an uppercase English letter.
- $0 \le n \le 100$

---

### 💡 Intuition & Strategy

1. **Greedy Frequency Distribution**:
   - The most frequent task dictates the layout of the schedule because it forces the maximum number of cooling intervals.
   - We count the occurrences of each task and identify the maximum frequency (`maxFreq`) along with how many tasks share this exact maximum frequency (`maxCount`).

2. **Constructing Frames & Idle Slots**:
   - The most frequent task creates $\text{maxFreq} - 1$ chunks or blocks, where each block must span a length of $n + 1$ to satisfy the cooling constraint.
   - The base length required by these frames plus the remaining tasks of maximum frequency is calculated as:
     $$\text{emptySlots} = (\text{maxFreq} - 1) \times (n + 1) + \text{maxCount}$$

3. **Handling High-Diversity Tasks**:
   - If there are many distinct tasks, the required schedule might not need any idle time because other tasks can comfortably fill the gaps.
   - In that case, the total number of tasks (`tasks.length`) will naturally be greater than the calculated formula. Thus, the minimum interval count is simply `Math.max(emptySlots, tasks.length)`.

4. **Complexity**:
   - **Time Complexity**: $O(N)$, where $N$ is the number of tasks. Counting frequencies takes $O(N)$ time, and sorting a constant-size array of 26 elements takes $O(1)$ time.
   - **Space Complexity**: $O(1)$ auxiliary memory, since the frequency array size is fixed at 26 uppercase English letters.

---

### 💻 Java Solution

```java
/**
 * Problem: Task Scheduler (LeetCode 621)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.Arrays;

class Solution {
    public int leastInterval(char[] tasks, int n) {
        // Frequency array for 26 uppercase English letters
        int[] freq = new int[26];
        for (char task : tasks) {
            freq[task - 'A']++;
        }
        
        // Sort the frequencies so the maximum frequency is at the end
        Arrays.sort(freq);
        
        int maxFreq = freq[25];
        int maxCount = 0;
        
        // Count how many tasks share the same maximum frequency
        for (int i = 25; i >= 0; i--) {
            if (freq[i] == maxFreq) {
                maxCount++;
            } else {
                break;
            }
        }
        
        // Calculate the result using the greedy slot formula
        int partCount = maxFreq - 1;
        int partLength = n + 1;
        int emptySlots = partCount * partLength + maxCount;
        
        // Return the maximum between the calculated slots and total tasks length
        return Math.max(emptySlots, tasks.length);
    }
}
