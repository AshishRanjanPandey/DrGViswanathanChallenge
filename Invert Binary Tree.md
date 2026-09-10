# 🎯 LeetCode 226: Invert Binary Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the root of a binary tree, invert the tree, and return its root.

#### Examples

- **Example 1**:
  - **Input**: `root = [4,2,7,1,3,6,9]`
  - **Output**: `[4,7,2,9,6,3,1]`
  - **Explanation**: The left subtree and right subtree of every single node in the tree are swapped with one another.

- **Example 2**:
  - **Input**: `root = [2,1,3]`
  - **Output**: `[2,3,1]`

- **Example 3**:
  - **Input**: `root = []`
  - **Output**: `[]`

#### Constraints

- The number of nodes in the tree is in the range $[0, 100]$.
- $-100 \le \text{Node.val} \le 100$

---

### 💡 Intuition & Strategy

1. **Core Property**:
   - Inverting a binary tree requires swapping the left and right child pointers for every node across the entire tree.
   - Once the children of a node are swapped, their corresponding subtrees must also be inverted recursively or iteratively.

2. **Depth-First Search (DFS) / Recursive Approach**:
   - **Base Case**: If the current node is `null`, return `null`.
   - **Action**: Swap `root.left` and `root.right` using a temporary reference pointer.
   - **Recurrence**: Call `invertTree(root.left)` and `invertTree(root.right)` to mirror the children recursively.
   - **Return**: Return the mutated `root`.

3. **Breadth-First Search (BFS) / Iterative Approach**:
   - Instead of call stack frames, use a `Queue<TreeNode>` to invert level-by-level.
   - For each dequeued node, swap its left and right children, then enqueue any non-null children.
   - Avoids call-stack overhead and prevents potential `StackOverflowError` on deeply skewed trees.

4. **Complexity Analysis**:
   - **Time Complexity**: $O(N)$
     - Every node is visited and swapped exactly once, where $N$ is the total number of nodes.
   - **Space Complexity**: $O(H)$
     - For the recursive approach, auxiliary space corresponds to the recursion call stack height $H$.
     - Best/Average Case (balanced tree): $O(\log N)$.
     - Worst Case (completely skewed degenerate tree): $O(N)$.

---

### 💻 Java Solution

```java
/**
 * Problem: Invert Binary Tree (LeetCode 226)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(H) where H is tree height
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

    public TreeNode invertTree(TreeNode root) {
        // Base case: empty subtree
        if (root == null) {
            return null;
        }

        // Swap the left and right children
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        // Recursively invert both subtrees
        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
}
