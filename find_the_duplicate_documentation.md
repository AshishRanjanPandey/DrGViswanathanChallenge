# 🎯 LeetCode 287: Find the Duplicate Number

> **Submission for**: #DrGVishwanathanChallenge

### 📝 Problem Description

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive. There is only one repeated number in `nums`, return this repeated number.

You must solve the problem **without** modifying the array `nums` and using only **constant extra space**.

#### Examples

* **Example 1**:
  * **Input**: `nums = [1,3,4,2,2]`
  * **Output**: `2`

* **Example 2**:
  * **Input**: `nums = [3,1,3,4,2]`
  * **Output**: `3`

* **Example 3**:
  * **Input**: `nums = [3,3,3,3,3]`
  * **Output**: `3`

#### Constraints

* $1 \le n \le 10^5$
* `nums.length == n + 1`
* $1 \le \text{nums}[i] \le n$
* All the integers in `nums` appear only once except for precisely one integer which appears two or more times.

---

### 💡 Intuition & Strategy

1. **Mapping to a Linked List Cycle**:
   * Because all values fall within the range $[1, n]$ and the array size is $n + 1$, each element can act as a pointer/index to the next position (`index -> nums[index]`).
   * The presence of a duplicate means two different indices point to the same target value, creating a cycle. This transforms the problem into finding the entry point of a cycle in a linked list.

2. **Floyd’s Tortoise and Hare (Cycle Detection)**:
   * **Phase 1 (Intersection)**: Initialize two pointers (`tortoise` and `hare`) at `nums[0]`. Move `tortoise` by 1 step (`nums[tortoise]`) and `hare` by 2 steps (`nums[nums[hare]]`) until they meet inside the cycle.
   * **Phase 2 (Cycle Entrance)**: Reset `tortoise` back to `nums[0]`. Move both `tortoise` and `hare` one step at a time (`nums[pointer]`). The node where they meet again is the entrance of the cycle, which is our duplicate number.

3. **Complexity**:
   * **Time Complexity**: $O(N)$
     * Both phases traverse the array at most a constant number of times relative to $N$.
   * **Space Complexity**: $O(1)$ auxiliary memory
     * Only two integer pointers are used, satisfying the constant extra space constraint without modifying the input array.

---

### 💻 Java Solution

```java
/**
 * Problem: Find the Duplicate Number (LeetCode 287)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */

class Solution {
    public int findDuplicate(int[] nums) {
        // Phase 1: Find the intersection point in the cycle
        int tortoise = nums[0];
        int hare = nums[0];
        
        do {
            tortoise = nums[tortoise];          // Moves 1 step
            hare = nums[nums[hare]];            // Moves 2 steps
        } while (tortoise != hare);
        
        // Phase 2: Find the entrance to the cycle (the duplicate number)
        tortoise = nums[0];
        while (tortoise != hare) {
            tortoise = nums[tortoise];          // Moves 1 step
            hare = nums[hare];                  // Moves 1 step
        }
        
        return hare;
    }
}
```