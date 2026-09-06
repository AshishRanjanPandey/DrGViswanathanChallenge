# 🎯 LeetCode 203: Remove Linked List Elements

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that have `Node.val == val`, and return the new head.

#### Examples

- **Example 1**:
  - **Input**: `head = [1, 2, 6, 3, 4, 5, 6]`, `val = 6`
  - **Output**: `[1, 2, 3, 4, 5]`

- **Example 2**:
  - **Input**: `head = []`, `val = 1`
  - **Output**: `[]`

- **Example 3**:
  - **Input**: `head = [7, 7, 7, 7]`, `val = 7`
  - **Output**: `[]`

#### Constraints

- The number of nodes in the list is in the range $[0, 10^4]$.
- $1 \le \text{Node.val} \le 50$
- $0 \le \text{val} \le 50$

---

### 💡 Intuition & Strategy

1. **The Head Removal Dilemma**:
   - In a singly linked list, deleting a non-head node requires adjusting the `next` pointer of its preceding node:
     $$\text{prev.next} = \text{curr.next}$$
   - When the target values reside at the very beginning of the list (e.g., `[7, 7, 7, 7]` with `val = 7`), standard node traversal requires extra logic to repeatedly reassign the `head` pointer.

2. **Sentinel / Dummy Head Pattern**:
   - By creating a `dummy` node such that `dummy.next = head`, the original `head` is treated like any intermediate node.
   - We maintain a pointer `curr` initialized to `dummy`. At each step:
     - If `curr.next.val == val`: skip the node via `curr.next = curr.next.next`.
     - Otherwise: move forward via `curr = curr.next`.
   - The method eliminates edge cases involving an empty list, a list where all nodes match `val`, or target nodes occurring at the head.

3. **In-Place Traversal**:
   - Only pointer references are mutated; no auxiliary data structures or newly allocated node copies are required.
   - Finally, returning `dummy.next` yields the correct, updated head of the list.

4. **Complexity**:
   - **Time Complexity**: $O(n)$ — We visit each of the $n$ nodes in the linked list at most once.
   - **Space Complexity**: $O(1)$ — Only a single dummy node and a traversal pointer are used, requiring constant auxiliary memory.

---

### 💻 Java Solution

```java
/**
 * Problem: Remove Linked List Elements (LeetCode 203)
 * Language: Java 17
 * Time Complexity: O(n)
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

    public ListNode removeElements(ListNode head, int val) {
        // Sentinel node to streamline head node deletions
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode curr = dummy;

        // Traverse list checking the next node's value
        while (curr.next != null) {
            if (curr.next.val == val) {
                // Bypass node containing matching target value
                curr.next = curr.next.next;
            } else {
                // Advance pointer only when no deletion occurred
                curr = curr.next;
            }
        }

        return dummy.next;
    }
}
```
### ⏱️ Detailed Complexity Analysis

| Metric | Dummy Node (Optimal) | Two Pointers (No Dummy) | Recursive Approach |
| :--- | :---: | :---: | :---: |
| **Best-Case Time** | $O(n)$ | $O(n)$ | $O(n)$ |
| **Worst-Case Time** | $O(n)$ | $O(n)$ | $O(n)$ |
| **Auxiliary Space** | $O(1)$ | $O(1)$ | $O(n)$ |
| **Call Stack Depth** | $O(1)$ | $O(1)$ | Up to $10^4$ frames |
| **Pointers Used** | `dummy`, `curr` | `prev`, `curr` | None (Implicit stack) |
| **Edge-Case Handling** | Uniform (Zero branches) | Needs leading loop for head | Clean base case |
