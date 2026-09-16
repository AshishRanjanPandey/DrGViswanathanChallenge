# 🎯 LeetCode 206: Reverse Linked List

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the `head` of a singly linked list, reverse the list, and return the reversed list.

#### Examples

- **Example 1**:
  - **Input**: `head = [1,2,3,4,5]`
  - **Output**: `[5,4,3,2,1]`

- **Example 2**:
  - **Input**: `head = [1,2]`
  - **Output**: `[2,1]`

- **Example 3**:
  - **Input**: `head = []`
  - **Output**: `[]`

#### Constraints

- The number of nodes in the list is in the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

---

### 💡 Intuition & Strategy

1. **Iterative In-Place Reversal (Three-Pointer Approach)**:
   - To reverse a singly linked list without extra memory, redirect each node's `next` pointer to point to its predecessor instead of its successor.
   - Because changing `curr.next` breaks access to the remaining list, store the reference to the next node before modifying the link.

2. **Pointer Maintenance**:
   - Maintain `prev` initialized to `null` (since the original head will become the tail pointing to `null`).
   - Maintain `curr` initialized to `head`.

3. **Step-by-Step Traversal**:
   - While `curr` is not `null`:
     - Temporarily store `curr.next` in `nextTemp`.
     - Invert the pointer: `curr.next = prev`.
     - Shift `prev` to `curr`.
     - Shift `curr` to `nextTemp`.
   - When the traversal finishes, `prev` will be pointing to the new head of the reversed list.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Traverses each of the $N$ nodes in the linked list exactly once.
   - **Space Complexity**: $O(1)$
     - Performs the reversal in-place using constant auxiliary pointer variables.

---

### 💻 Java Solution

```java
/**
 * Problem: Reverse Linked List (LeetCode 206)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1)
 * Challenge: Dr. G. Vishwanathan Challenge
 * 
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

    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;

        while (curr != null) {
            ListNode nextTemp = curr.next; // Store next node
            curr.next = prev;             // Reverse pointer
            prev = curr;                  // Advance prev
            curr = nextTemp;              // Advance curr
        }

        return prev;
    }
}
