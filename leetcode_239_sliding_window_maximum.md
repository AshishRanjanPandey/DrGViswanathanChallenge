# 🎯 LeetCode 239: Sliding Window Maximum

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

#### Examples

- **Example 1**:
  - **Input**: `nums = [1,3,-1,-3,5,3,6,7]`, `k = 3`
  - **Output**: `[3,3,5,5,6,7]`
  - **Explanation**: 
    ```text
    Window position                Max
    ---------------               -----
    [1  3  -1] -3  5  3  6  7       3
     1 [3  -1  -3] 5  3  6  7       3
     1  3 [-1  -3  5] 3  6  7       5
     1  3  -1 [-3  5  3] 6  7       5
     1  3  -1  -3 [5  3  6] 7       6
     1  3  -1  -3  5 [3  6  7]      7
    ```

- **Example 2**:
  - **Input**: `nums = [1]`, `k = 1`
  - **Output**: `[1]`

#### Constraints

- $1 \le \text{nums.length} \le 10^5$
- $-10^4 \le \text{nums}[i] \le 10^4$
- $1 \le k \le \text{nums.length}$

---

### 💡 Intuition & Strategy

1. **The Monotonic Deque**:
   - A naive approach of finding the maximum in every window of size $k$ takes $O(N \times k)$ time, which will lead to a Time Limit Exceeded (TLE) error.
   - We can optimize this by using a **Double-Ended Queue (Deque)** to store the *indices* of the array elements. The deque will be kept in a strictly monotonically decreasing order of the elements they represent.

2. **Window Sliding & Cleaning (Front)**:
   - As our loop index `i` moves forward, the sliding window shifts. If the index at the front of the deque falls outside our current window (i.e., `deque.peekFirst() < i - k + 1`), we pop it off from the front.

3. **Removing "Useless" Elements (Back)**:
   - Before adding the current element `nums[i]` to the deque, we check the elements at the back of the deque.
   - If `nums[i]` is greater than the elements currently at the back, those smaller elements are completely useless. Why? Because `nums[i]` is both *larger* and appears *later* in the array, meaning it will survive longer in the sliding window. We pop them off from the back.

4. **Recording the Maximum**:
   - Because of the cleanup in step 3, the largest element in the current window will always naturally float to the front of the deque.
   - Once our window has reached at least size $k$ (when `i >= k - 1`), we record the element corresponding to the front index into our result array.

5. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Every element is pushed into the deque exactly once and popped out at most once. The amortized time cost per element is $O(1)$.
   - **Space Complexity**: $O(k)$ auxiliary memory
     - The deque stores at most $k$ indices at any given time.

---

### 💻 Java Solution

```java
/**
 * Problem: Sliding Window Maximum (LeetCode 239)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(k) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length == 0 || k <= 0) {
            return new int[0];
        }
        
        int n = nums.length;
        int[] result = new int[n - k + 1];
        int resultIndex = 0;
        
        // The deque stores the INDICES of the array elements.
        // It maintains a strictly decreasing order of the corresponding elements.
        Deque<Integer> deque = new ArrayDeque<>();
        
        for (int i = 0; i < n; i++) {
            // 1. Remove indices that are out of the current window bounds
            while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
                deque.pollFirst();
            }
            
            // 2. Remove indices of elements smaller than the current element
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }
            
            // 3. Add the current element's index to the deque
            deque.offerLast(i);
            
            // 4. If our window has hit size k, add the max (at the front) to the result
            if (i >= k - 1) {
                result[resultIndex++] = nums[deque.peekFirst()];
            }
        }
        
        return result;
    }
}
```