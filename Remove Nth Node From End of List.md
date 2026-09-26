# 🎯 LeetCode 19: Remove Nth Node From End of List

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the head of a linked list, remove the $n$-th node from the end of the list and return its head.

#### Examples

- **Example 1**:
  - **Input**: `head = [1,2,3,4,5]`, `n = 2`
  - **Output**: `[1,2,3,5]`
  - **Explanation**: The 2nd node from the end is `4`, which is removed.

- **Example 2**:
  - **Input**: `head = [1]`, `n = 1`
  - **Output**: `[]`
  - **Explanation**: The only node in the list is removed, leaving it empty.

- **Example 3**:
  - **Input**: `head = [1,2]`, `n = 1`
  - **Output**: `[1]`
  - **Explanation**: The 1st node from the end is `2`, which is removed.

#### Constraints

- The number of nodes in the list is $sz$.
- $1 \le sz \le 30$
- $0 \le \text{Node.val} \le 100$
- $1 \le n \le sz$

---

### 💡 Intuition & Strategy

1. **Two-Pointer (Fast and Slow) Technique**:
   - To remove the $n$-th node from the end in a **single pass**, we can maintain a fixed gap of $n$ nodes between two pointers (`fast` and `slow`).

2. **Using a Dummy Node**:
   - We introduce a `dummy` node pointing to the `head`. This acts as a safety guard, making it trivial to handle edge cases like removing the actual head of the list without needing extra conditional checks.

3. **Step-by-Step Execution**:
   - Initialize both `fast` and `slow` pointers at the `dummy` node.
   - Advance the `fast` pointer $n + 1$ steps ahead so that the distance between `fast` and `slow` covers $n$ nodes.
   - Traverse the list by moving both `fast` and `slow` pointers concurrently one step at a time until `fast` reaches the end of the list (`null`).
   - At this point, `slow` will be positioned right before the target node. We simply bypass the target node by setting `slow.next = slow.next.next`.

4. **Complexity**:
   - **Time Complexity**: $O(L)$
     - Where $L$ is the number of nodes in the linked list. The `fast` pointer traverses the list exactly once.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Only a constant amount of extra space is used for the pointers.

---

### 💻 Java Solution

```java
/**
 * Problem: Remove Nth Node From End of List (LeetCode 19)
 * Language: Java 17
 * Time Complexity: O(L)
 * Space Complexity: O(1)
 * Challenge: Dr. G. Vishwanathan Challenge
 */

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Create a dummy node to handle edge cases (like removing the head)
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode fast = dummy;
        ListNode slow = dummy;
        
        // Move fast n + 1 steps ahead so there is a gap of n nodes between fast and slow
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }
        
        // Move both pointers until fast reaches the end of the list
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        
        // Skip the target node
        slow.next = slow.next.next;
        
        return dummy.next;
    }
}
