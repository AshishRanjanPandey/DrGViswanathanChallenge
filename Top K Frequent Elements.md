# 🎯 LeetCode 347: Top K Frequent Elements

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

#### Examples

- **Example 1**:
  - **Input**: `nums = [1,1,1,2,2,3]`, `k = 2`
  - **Output**: `[1,2]`
  - **Explanation**: `1` occurs three times, `2` occurs two times, and `3` occurs once. The two most frequent elements are `1` and `2`.

- **Example 2**:
  - **Input**: `nums = [1]`, `k = 1`
  - **Output**: `[1]`

- **Example 3**:
  - **Input**: `nums = [1,2,1,2,1,2,3,1,3,2]`, `k = 2`
  - **Output**: `[1,2]`

#### Constraints

- $1 \le \text{nums.length} \le 10^5$
- $-10^4 \le \text{nums}[i] \le 10^4$
- `k` is in the range $[1, \text{the number of unique elements in the array}]$.
- It is guaranteed that the answer is unique.

#### Follow-up
- Your algorithm's time complexity must be better than $O(N \log N)$, where $N$ is the array's size.

---

### 💡 Intuition & Strategy

1. **Frequency Mapping**:
   - Count the frequency of each unique number using a Hash Map (`Map<Integer, Integer>`).
   - Computing counts takes $O(N)$ time.

2. **Bucket Sort (Optimal Linear Approach)**:
   - Traditional sorting of unique frequencies takes $O(U \log U)$, where $U$ is the number of unique elements.
   - A Min-Heap bound to size $k$ achieves $O(N \log k)$ time.
   - To achieve strictly better performance ($O(N)$ linear time), we can leverage **Bucket Sort**:
     - The maximum frequency an element can have is $N$.
     - Create an array of lists `buckets` of size $N + 1$, where index $i$ stores all elements that appear exactly $i$ times.

3. **Collection**:
   - Traverse the bucket array from the highest frequency index ($N$) down to $1$.
   - Collect elements until we have populated our result array with $k$ items.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Building frequency map: $O(N)$.
     - Distributing into buckets: $O(U) \le O(N)$.
     - Reverse bucket iteration: $O(N)$ worst-case to inspect buckets.
     - Total time: $O(N)$, which strictly satisfies the follow-up requirement.
   - **Space Complexity**: $O(N)$
     - The frequency map and bucket array both scale linearly with the input size.

---

### 💻 Java Solution

```java
/**
 * Problem: Top K Frequent Elements (LeetCode 347)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {

    public int[] topKFrequent(int[] nums, int k) {
        // Step 1: Count frequency of each number
        Map<Integer, Integer> frequencyMap = new HashMap<>();
        for (int num : nums) {
            frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
        }

        // Step 2: Group elements by their frequency into buckets
        // Index represents frequency, bucket contents are the numbers with that frequency
        @SuppressWarnings("unchecked")
        List<Integer>[] buckets = new ArrayList[nums.length + 1];

        for (Map.Entry<Integer, Integer> entry : frequencyMap.entrySet()) {
            int num = entry.getKey();
            int freq = entry.getValue();

            if (buckets[freq] == null) {
                buckets[freq] = new ArrayList<>();
            }
            buckets[freq].add(num);
        }

        // Step 3: Collect top k elements starting from highest frequency
        int[] result = new int[k];
        int writeIndex = 0;

        for (int freq = buckets.length - 1; freq >= 1 && writeIndex < k; freq--) {
            if (buckets[freq] != null) {
                for (int num : buckets[freq]) {
                    result[writeIndex++] = num;
                    if (writeIndex == k) {
                        break;
                    }
                }
            }
        }

        return result;
    }
}
