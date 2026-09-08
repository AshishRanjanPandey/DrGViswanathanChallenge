# 🎯 LeetCode 100: Same Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

#### Examples

- **Example 1**:
  - **Input**: `p = [1,2,3], q = [1,2,3]`
  - **Output**: `true`
  - **Explanation**: Both trees have identical node values and structure.

- **Example 2**:
  - **Input**: `p = [1,2], q = [1,null,2]`
  - **Output**: `false`
  - **Explanation**: The structures differ; node `2` is a left child in `p` and a right child in `q`.

- **Example 3**:
  - **Input**: `p = [1,2,1], q = [1,1,2]`
  - **Output**: `false`
  - **Explanation**: Structures are identical, but corresponding node values differ (`2 != 1`).

#### Constraints

- The number of nodes in both trees is in the range $[0, 100]$.
- $-10^4 \le \text{Node.val} \le 10^4$

---

### 💡 Intuition & Strategy

1. **Recursive Invariance (Divide and Conquer)**:
   - Two trees are identical if and only if:
     1. Their root values are identical.
     2. Their left subtrees are identical.
     3. Their right subtrees are identical.
   - This hierarchical relationship naturally leads to a recursive Depth-First Search (DFS) traversal.

2. **Base Cases**:
   - **Both Null**: If both `p == null` and `q == null`, both branches have terminated without discrepancies $\rightarrow$ return `true`.
   - **Structural Mismatch**: If one node is `null` while the other is not (`p == null || q == null`), the tree structures diverge $\rightarrow$ return `false`.
   - **Value Mismatch**: If both nodes exist but `p.val != q.val`, their values diverge $\rightarrow$ return `false`.

3. **Recursive Step**:
   - If the current node values match, recurse on both subtrees simultaneously:
     `isSameTree(p.left, q.left) && isSameTree(p.right, q.right)`

4. **Complexity**:
   - **Time Complexity**: $O(N)$, where $N$ is the number of nodes in the smaller tree. Each node is visited at most once.
   - **Space Complexity**: $O(H)$, where $H$ is the height of the tree, corresponding to the recursion call stack depth ($O(\log N)$ best/average case for balanced trees, $O(N)$ worst case for completely skewed trees).

---

### 💻 Java Solution

```java
/**
 * Problem: Same Tree (LeetCode 100)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(H)
 * Challenge: Dr. G. Vishwanathan Challenge
 *
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public boolean isSameTree(TreeNode p, TreeNode q) {
        // Case 1: Both nodes are null -> structurally identical at this leaf
        if (p == null && q == null) {
            return true;
        }

        // Case 2: One node is null while the other is not -> structural mismatch
        if (p == null || q == null) {
            return false;
        }

        // Case 3: Node values do not match -> value mismatch
        if (p.val != q.val) {
            return false;
        }

        // Case 4: Values match -> evaluate left and right subtrees recursively
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
