# 🎯 LeetCode 543: Diameter of Binary Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the `root` of a binary tree, return the length of the **diameter** of the tree.

The **diameter** of a binary tree is the **length of the longest path** between any two nodes in a tree. This path may or may not pass through the `root`.

The length of a path between two nodes is represented by the number of **edges** between them.

#### Examples

- **Example 1**:
  - **Input**: `root = [1,2,3,4,5]`
  - **Output**: `3`
  - **Explanation**: 3 is the length of the path `[4,2,1,3]` or `[5,2,1,3]`.

- **Example 2**:
  - **Input**: `root = [1,2]`
  - **Output**: `1`
  - **Explanation**: The path consists of the single edge connecting `1` and `2`.

#### Constraints

- The number of nodes in the tree is in the range `[1, 10^4]`.
- `-100 <= Node.val <= 100`

---

### 💡 Intuition & Strategy

1. **Path Representation through an Apex**:
   - Any path between two nodes in a binary tree reaches a highest common ancestor (an apex node) and extends downwards into its left and right subtrees.
   - The maximum number of edges on a path passing through a given node as its highest point is:
     `diameter_at_node = height(left child) + height(right child)`
   - Here, the height of an empty child (`null`) is defined as `0`, and each node adds `1` to the path extending upward.

2. **Bottom-Up Depth-First Search (Post-Order Traversal)**:
   - A naive approach would recompute subtree heights at every node, taking $O(N^2)$ time.
   - Instead, compute the height of subtrees bottom-up using post-order DFS (`Left -> Right -> Node`).
   - At each node:
     1. Recursively calculate `leftHeight` and `rightHeight`.
     2. Update the running global maximum diameter: `maxDiameter = max(maxDiameter, leftHeight + rightHeight)`.
     3. Return `1 + max(leftHeight, rightHeight)` up to the caller so the parent can evaluate its own paths.

3. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Every node in the binary tree is visited exactly once during the post-order traversal.
   - **Space Complexity**: $O(H)$ auxiliary memory
     - $H$ corresponds to the height of the tree, representing the recursion call stack overhead.
     - In the best/balanced case, $H = O(\log N)$.
     - In the worst case (completely skewed degenerate tree), $H = O(N)$.

---

### 💻 Java Solution

```java
/**
 * Problem: Diameter of Binary Tree (LeetCode 543)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(H) auxiliary recursion stack
 * Challenge: Dr. G. Vishwanathan Challenge
 */

/**
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
    private int maxDiameter;

    public int diameterOfBinaryTree(TreeNode root) {
        maxDiameter = 0;
        calculateHeight(root);
        return maxDiameter;
    }

    private int calculateHeight(TreeNode node) {
        if (node == null) {
            return 0;
        }

        // Post-order: resolve subtrees first
        int leftHeight = calculateHeight(node.left);
        int rightHeight = calculateHeight(node.right);

        // Path passing through this node as apex
        maxDiameter = Math.max(maxDiameter, leftHeight + rightHeight);

        // Return height contributed to parent node
        return 1 + Math.max(leftHeight, rightHeight);
    }
}
