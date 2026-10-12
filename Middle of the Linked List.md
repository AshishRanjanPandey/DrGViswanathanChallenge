# 🎯 LeetCode 876: Middle of the Linked List

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the head of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

#### Examples

- **Example 1**:
  - **Input**: `head = [1,2,3,4,5]`
  - **Output**: `[3,4,5]`
  - **Explanation**: The middle node of the list is node 3 (value `3`).

- **Example 2**:
  - **Input**: `head = [1,2,3,4,5,6]`
  - **Output**: `[4,5,6]`
  - **Explanation**: Since the list has two middle nodes with values `3` and `4`, we return the second one (node 4).

#### Constraints

- The number of nodes in the list is in the range $[1, 100]$.
- $1 \le \text{Node.val} \le 100$

---

### 💡 Intuition & Strategy

1. **The Two-Pointer (Tortoise and Hare) Technique**:
   - To find the middle of a linked list in a single pass without calculating its length beforehand, we can use two pointers: `slow` and `fast`.
   - Both pointers start at the `head` of the linked list.

2. **Pace Differential**:
   - The **`slow` pointer** moves forward by **1 step** at a time (`slow = slow.next`).
   - The **`fast` pointer** moves forward by **2 steps** at a time (`fast = fast.next.next`).

3. **Natural Termination & Even/Odd Handling**:
   - Because `fast` moves twice as fast as `slow`, by the time `fast` reaches the end of the list (`null` for odd-length lists or the last node's `next` for even-length lists), `slow` will have traversed exactly half the distance.
   - For an even number of nodes, this automatically lands `slow` on the **second** middle node as required.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - We traverse the linked list at most once, where $N$ is the total number of nodes.
   - **Space Complexity**: $O(1)$ auxiliary memory
     - Only two pointer references are used, requiring constant extra space.

---

### 💻 Java Solution

```java
/**
 * Problem: Middle of the Linked List (LeetCode 876)
 * Language: Java 17
 * Time Complexity: O(N)
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
    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        // Traverse the list until 'fast' reaches the end
        while (fast != null && fast.next != null) {
            slow = slow.next;          // Move 1 step
            fast = fast.next.next;     // Move 2 steps
        }
        
        // When fast hits the end, slow is positioned at the middle node
        return slow;
    }
}
