# 🎯 LeetCode 114: Flatten Binary Tree to Linked List

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the root of a binary tree, flatten the tree into a "linked list":
- The "linked list" should use the same `TreeNode` class where the right child pointer points to the next node in the list and the left child pointer is always `null`.
- The "linked list" should be in the same order as a pre-order traversal of the binary tree.

#### Examples

- **Example 1**:
  - **Input**: `root = [1,2,5,3,4,null,6]`
  - **Output**: `[1,null,2,null,3,null,4,null,5,null,6]`

- **Example 2**:
  - **Input**: `root = []`
  - **Output**: `[]`

- **Example 3**:
  - **Input**: `root = [0]`
  - **Output**: `[0]`

#### Constraints

- The number of nodes in the tree is in the range $[0, 2000]$.
- $-100 \le \text{Node.val} \le 100$

---

### 💡 Intuition & Strategy

1. **Morris-style In-place Traversal**:
   - Instead of using recursion or an explicit stack (which takes $O(N)$ auxiliary space), we can achieve an optimal $O(1)$ space solution.
   - For each node, if it has a left child, we locate the rightmost node of its left subtree (which serves as the pre-order predecessor).

2. **Re-wiring Pointers**:
   - We connect the current node's right subtree to the right pointer of this predecessor node.
   - We then move the entire left subtree to the right child pointer of the current node and set the left child pointer to `null`.
   - Finally, we advance to the next node via `curr = curr.right`.

3. **Complexity**:
   - **Time Complexity**: $O(N)$, where $N$ is the number of nodes in the binary tree. Every node is visited a constant number of times.
   - **Space Complexity**: $O(1)$ auxiliary memory since no stack or recursive calls are utilized.

---

### 💻 Java Solution

```java
/**
 * Problem: Flatten Binary Tree to Linked List (LeetCode 114)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(1) auxiliary
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public void flatten(TreeNode root) {
        TreeNode curr = root;
        
        while (curr != null) {
            // If the node has a left child, we need to process it
            if (curr.left != null) {
                // Find the rightmost node of the left subtree (predecessor)
                TreeNode predecessor = curr.left;
                while (predecessor.right != null) {
                    predecessor = predecessor.right;
                }
                
                // Connect the current node's right subtree to the rightmost node of the left subtree
                predecessor.right = curr.right;
                
                // Move the left subtree to the right side and clear the left pointer
                curr.right = curr.left;
                curr.left = null;
            }
            
            // Move to the next node on the right
            curr = curr.right;
        }
    }
}
```
