# 🎯 LeetCode 238: Product of Array Except Self

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a **32-bit** integer.

You must write an algorithm that runs in $O(N)$ time and without using the division operation.

#### Examples

- **Example 1**:
  - **Input**: `nums = [1,2,3,4]`
  - **Output**: `[24,12,8,6]`
  - **Explanation**: 
    - For index 0: $2 \times 3 \times 4 = 24$
    - For index 1: $1 \times 3 \times 4 = 12$
    - For index 2: $1 \times 2 \times 4 = 8$
    - For index 3: $1 \times 2 \times 3 = 6$

- **Example 2**:
  - **Input**: `nums = [-1,1,0,-3,3]`
  - **Output**: `[0,0,9,0,0]`

#### Constraints

- $2 \le \text{nums.length} \le 10^5$
- $-30 \le \text{nums}[i] \le 30$
- The product of any prefix or suffix of `nums` is guaranteed to fit in a **32-bit** integer.

#### Follow-up

Can you solve the problem in $O(1)$ extra space complexity? (The output array does not count as extra space for space complexity analysis.)

---

### 💡 Intuition & Strategy

1. **Prefix and Suffix Decomposition**:
   - For any index $i$, the total product except `nums[i]` is:
     $$\text{answer}[i] = (\text{product of all elements to the left of } i) \times (\text{product of all elements to the right of } i)$$
   - Division is strictly disallowed, so total product division by `nums[i]` cannot be used (which also fails when elements contain zeros).

2. **$O(1)$ Auxiliary Space Optimization**:
   - Instead of allocating two separate prefix and suffix arrays of size $N$, reuse the output array `answer` to accumulate prefix products in a forward pass.
   - Run a second pass from right to left using a single scalar variable `rightProduct` to dynamically compute and multiply the suffix product into `answer[i]`.

3. **Algorithm Walkthrough**:
   - **Forward Pass**: Initialize `answer[0] = 1`. For every $i$ from $1$ to $N - 1$, set `answer[i] = answer[i - 1] * nums[i - 1]`.
   - **Backward Pass**: Initialize a scalar variable `rightProduct = 1`. For every $i$ from $N - 1$ down to $0$:
     - Multiply `answer[i]` by `rightProduct`.
     - Update `rightProduct *= nums[i]`.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Two consecutive linear passes over the array of size $N$.
   - **Space Complexity**: $O(1)$
     - Output array `answer` is excluded by problem constraints; strictly uses a single scalar variable for suffix state tracking.

---

### 💻 Java Solution

```java
/**
 * Problem: Product of Array Except Self (LeetCode 238)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1) (Auxiliary)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {

    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] answer = new int[n];

        // Step 1: Compute prefix products directly in the output array.
        // answer[i] stores the product of all elements to the left of index i.
        answer[0] = 1;
        for (int i = 1; i < n; i++) {
            answer[i] = answer[i - 1] * nums[i - 1];
        }

        // Step 2: Backward pass using a scalar variable to multiply suffix products.
        int rightProduct = 1;
        for (int i = n - 1; i >= 0; i--) {
            answer[i] *= rightProduct;
            rightProduct *= nums[i];
        }

        return answer;
    }
}
