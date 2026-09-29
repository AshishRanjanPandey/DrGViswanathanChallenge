# 🎯 LeetCode 21: Merge Two Sorted Lists

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

#### Examples

- **Example 1**:
  - **Input**: `list1 = [1,2,4], list2 = [1,3,4]`
  - **Output**: `[1,1,2,3,4,4]`

- **Example 2**:
  - **Input**: `list1 = [], list2 = []`
  - **Output**: `[]`

- **Example 3**:
  - **Input**: `list1 = [], list2 = [0]`
  - **Output**: `[0]`

#### Constraints

- The number of nodes in both lists is in the range $[0, 50]$.
- $-100 \le \text{Node.val} \le 100$
- Both `list1` and `list2` are sorted in non-decreasing order.

---

### 💡 Intuition & Strategy

1. **Using a Dummy Node**:
   - Since the head of the final merged list can change depending on which list has the smaller initial value, using a **dummy node** helps simplify pointer manipulation and avoids special-case handling for the head.
   - We maintain a `current` pointer starting at the dummy node to build our sorted list iteratively.

2. **Pointer Traversal & Comparison**:
   - While both `list1` and `list2` are non-empty, compare their current node values (`list1.val` vs `list2.val`).
   - Attach the node with the smaller value to `current.next`, then advance that list's pointer along with the `current` pointer.

3. **Attaching the Remainder**:
   - Once one of the lists is exhausted, the remaining nodes in the other list are already sorted. We can directly attach the non-null list to `current.next`.

4. **Complexity**:
   - **Time Complexity**: $O(n + m)$, where $n$ and $m$ are the lengths of `list1` and `list2`. We traverse both lists at most once.
   - **Space Complexity**: $O(1)$ auxiliary memory since we only use pointers (`dummy`, `current`) to rearrange existing nodes without extra data structures.

---

### 💻 Java Solution

```java
/**
 * Problem: Merge Two Sorted Lists (LeetCode 21)
 * Language: Java 17
 * Time Complexity: O(n + m)
 * Space Complexity: O(1) auxiliary
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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(-1);
        ListNode current = dummy;
        
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                current.next = list1;
                list1 = list1.next;
            } else {
                current.next = list2;
                list2 = list2.next;
            }
            current = current.next;
        }
        
        // Attach the remaining nodes from whichever list is not empty
        if (list1 != null) {
            current.next = list1;
        } else {
            current.next = list2;
        }
        
        return dummy.next;
    }
}
