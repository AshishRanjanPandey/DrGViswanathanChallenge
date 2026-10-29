# 🎯 LeetCode 2: Add Two Numbers

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number $0$ itself.

#### Examples

- **Example 1**:
  - **Input**: `l1 = [2,4,3], l2 = [5,6,4]`
  - **Output**: `[7,0,8]`
  - **Explanation**: $342 + 465 = 807$.

- **Example 2**:
  - **Input**: `l1 = [0], l2 = [0]`
  - **Output**: `[0]`

- **Example 3**:
  - **Input**: `l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]`
  - **Output**: `[8,9,9,9,0,0,0,1]`

#### Constraints

- The number of nodes in each linked list is in the range $[1, 100]$.
- $0 \le \text{Node.val} \le 9$
- It is guaranteed that the list represents a number that does not have leading zeros.

---

### 💡 Intuition & Strategy

1. **Dummy Head Initialization**:
   - Using a `dummyHead` simplifies linked list operations by giving us a fixed starting point. We don't have to write special edge cases for initializing the head of our result list.

2. **Single-Pass Iteration with Carry**:
   - We can traverse both linked lists simultaneously in a single `while` loop. 
   - The loop continues as long as `l1` is not null, `l2` is not null, or there is an active `carry`. This elegantly handles lists of unequal lengths and cases where the final sum generates an extra digit (e.g., $5 + 5 = 10$).

3. **Digit Extraction & Summation**:
   - For each step, safely extract the node values. If a list is exhausted, treat its missing digits as $0$.
   - Calculate the total sum for that positional column: `sum = x + y + carry`.
   - The new node's value is the last digit of the sum (`sum % 10`), and the new carry for the next column is the first digit (`sum / 10`).

4. **Complexity**:
   - **Time Complexity**: $O(\max(N, M))$
     - Where $N$ and $M$ are the lengths of `l1` and `l2`. We iterate at most $\max(N, M) + 1$ times.
   - **Space Complexity**: $O(\max(N, M))$
     - The length of the new resulting linked list is at most $\max(N, M) + 1$.

---

### 💻 Java Solution

```java
/**
 * Problem: Add Two Numbers (LeetCode 2)
 * Language: Java
 * Time Complexity: O(max(N, M))
 * Space Complexity: O(max(N, M))
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode current = dummyHead;
        int carry = 0;
        
        // Continue looping if there are remaining nodes in l1, l2, or an active carry
        while (l1 != null || l2 != null || carry != 0) {
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;
            
            int sum = carry + x + y;
            carry = sum / 10;
            
            current.next = new ListNode(sum % 10);
            current = current.next;
            
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }
        
        return dummyHead.next;
    }
}
```
