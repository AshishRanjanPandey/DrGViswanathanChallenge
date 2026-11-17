# 🎯 LeetCode 23: Merge k Sorted Lists

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

You are given an array of $k$ linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

#### Examples

- **Example 1**:
  - **Input**: `lists = [[1,4,5],[1,3,4],[2,6]]`
  - **Output**: `[1,1,2,3,4,4,5,6]`
  - **Explanation**: The linked-lists are:
    [
      1->4->5,
      1->3->4,
      2->6
    ]
    merging them into one sorted linked list:
    1->1->2->3->4->4->5->6

- **Example 2**:
  - **Input**: `lists = []`
  - **Output**: `[]`

- **Example 3**:
  - **Input**: `lists = [[]]`
  - **Output**: `[]`

#### Constraints

- $k == \text{lists.length}$
- $0 \le k \le 10^4$
- $0 \le \text{lists}[i].length \le 500$
- $-10^4 \le \text{lists}[i][j] \le 10^4$
- $\text{lists}[i]$ is sorted in ascending order.
- The sum of $\text{lists}[i].length$ will not exceed $10^4$.

---

### 💡 Intuition & Strategy

1. **Using a Min-Heap (Priority Queue)**:
  - Since we have $k$ sorted lists, at any given moment, the next smallest element in the merged list must be one of the current heads of the $k$ lists.
  - A Min-Heap (Priority Queue) efficiently keeps track of the smallest element among these $k$ heads.

2. **Initialization & Population**:
  - Create a dummy node to act as the starting point of our merged list, and a `current` pointer to build the list.
  - Iterate through the array of linked-lists and insert the head node of each non-empty list into the Min-Heap. The heap orders them based on their node values.

3. **Building the Merged List**:
  - While the Min-Heap is not empty:
    - Extract the node with the smallest value (the root of the heap).
    - Append this node to the merged list via the `current` pointer.
    - If the extracted node has a subsequent node (`node.next != null`), insert this next node into the Min-Heap.
    - Advance the `current` pointer.

4. **Complexity**:
  - **Time Complexity**: $O(N \log k)$
    - $N$ is the total number of nodes across all lists. Inserting and extracting from a Priority Queue of size $k$ takes $O(\log k)$ time. We do this for every single node.
  - **Space Complexity**: $O(k)$ auxiliary memory
    - The Priority Queue holds at most $k$ nodes (one from each list) at any given time. The rest of the space is just pointers, requiring $O(1)$ extra space.

---

### 💻 Java Solution

```java
/**
- Problem: Merge k Sorted Lists (LeetCode 23)
- Language: Java 17
- Time Complexity: O(N log k)
- Space Complexity: O(k) auxiliary (for the Priority Queue)
- Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.PriorityQueue;

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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }
        
        // Min-heap to keep track of the smallest current node across all lists
        PriorityQueue<ListNode> minHeap = new PriorityQueue<>(
            lists.length, 
            (a, b) -> Integer.compare(a.val, b.val)
        );
        
        // Add the head of each linked list to the priority queue
        for (ListNode node : lists) {
            if (node != null) {
                minHeap.add(node);
            }
        }
        
        // Dummy node to easily return the head of our merged list
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        
        // Build the merged list
        while (!minHeap.isEmpty()) {
            // Get the node with the smallest value
            ListNode smallestNode = minHeap.poll();
            current.next = smallestNode;
            current = current.next;
            
            // If there is a next node in the list we just pulled from, add it to the heap
            if (smallestNode.next != null) {
                minHeap.add(smallestNode.next);
            }
        }
        
        return dummy.next;
    }
}
```
