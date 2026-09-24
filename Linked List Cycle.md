# 🎯 LeetCode 141: Linked List Cycle

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's next pointer is connected to. Note that `pos` is not passed as a parameter.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

#### Examples

- **Example 1**:
  - **Input**: `head = [3,2,0,-4], pos = 1`
  - **Output**: `true`
  - **Explanation**: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

- **Example 2**:
  - **Input**: `head = [1,2], pos = 0`
  - **Output**: `true`
  - **Explanation**: There is a cycle in the linked list, where the tail connects to the 0th node.

- **Example 3**:
  - **Input**: `head = [1], pos = -1`
  - **Output**: `false`
  - **Explanation**: There is no cycle in the linked list.

#### Constraints

- The number of the nodes in the list is in the range $[0, 10^4]$.
- $-10^5 \le \text{Node.val} \le 10^5$
- `pos` is `-1` or a valid index in the linked-list.

---

### 💡 Intuition & Strategy

1. **Floyd's Cycle-Finding Algorithm (Tortoise and Hare)**:
   - Use two pointers starting at the `head`: `slow` (tortoise) and `fast` (hare).
   - The `slow` pointer advances by $1$ step, while the `fast` pointer advances by $2$ steps in each iteration.

2. **Cycle Detection Mechanism**:
   - If the linked list has an end (no cycle), the `fast` pointer will eventually reach `null` or have a `null` next reference, safely terminating the loop with a `false` result.
   - If a cycle exists, the faster pointer will loop around and catch up to the slower pointer inside the cycle, making `slow == fast` and returning `true`.

3. **Complexity**:
   - **Time Complexity**: $O(N)$, where $N$ is the number of nodes in the linked list. In the worst case, the fast pointer traverses the cycle a proportional number of times before meeting the slow pointer.
   - **Space Complexity**: $O(1)$ auxiliary memory since only two pointer references (`slow` and `fast`) are stored regardless of list size.

---

### 💻 Java Solution

```java
/**
 * Problem: Linked List Cycle (LeetCode 141)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */

/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        // Base case: if the list is empty or has only one node without a cycle
        if (head == null || head.next == null) {
            return false;
        }
        
        ListNode slow = head;
        ListNode fast = head;
        
        // Traverse the linked list with two pointers at different speeds
        while (fast != null && fast.next != null) {
            slow = slow.next;          // Tortoise moves 1 step
            fast = fast.next.next;     // Hare moves 2 steps
            
            // If they meet, there is a cycle
            if (slow == fast) {
                return true;
            }
        }
        
        // If fast reaches the end (null), there is no cycle
        return false;
    }
}
