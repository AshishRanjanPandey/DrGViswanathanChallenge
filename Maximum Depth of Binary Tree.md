# 🎯 LeetCode 104: Maximum Depth of Binary Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the `root` of a binary tree, return its maximum depth.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

#### Examples

- **Example 1**:
  - **Input**: `root = [3,9,20,null,null,15,7]`
  - **Output**: `3`
  - **Explanation**: The longest path from root `3` is either `3 -> 20 -> 15` or `3 -> 20 -> 7`, both containing 3 nodes.

- **Example 2**:
  - **Input**: `root = [1,null,2]`
  - **Output**: `2`
  - **Explanation**: The path is `1 -> 2`, which consists of 2 nodes.

#### Constraints

- The number of nodes in the tree is in the range $[0, 10^4]$.
- $-100 \le \text{Node.val} \le 100$

---

### 💡 Intuition & Strategy

1. **Recursive Decomposition (Divide and Conquer / DFS)**:
   - The depth of a binary tree rooted at `root` is fundamentally defined by the maximum depth of its subtrees plus $1$ for the root itself:
     $$\text{depth}(root) = 1 + \max(\text{depth}(root.left), \text{depth}(root.right))$$
   - This natural recursive relationship makes Depth-First Search (DFS) the most clean and optimal approach.

2. **Base Case**:
   - If the current node is `null` (an empty tree or empty subtree), its depth is `0`.

3. **Recursive Step**:
   - Recursively compute the maximum depth of the left child: `leftDepth = maxDepth(root.left)`.
   - Recursively compute the maximum depth of the right child: `rightDepth = maxDepth(root.right)`.
   - Aggregate the answer: return `1 + Math.max(leftDepth, rightDepth)`.

4. **Complexity Analysis**:
   - **Time Complexity**: $O(N)$
     - Every node in the binary tree is visited exactly once.
   - **Space Complexity**: $O(H)$
     - Where $H$ is the height of the binary tree, representing the recursion stack depth.
     - Best/Average Case (Balanced Tree): $O(\log N)$
     - Worst Case (Completely Skewed Tree): $O(N)$

---

### 💻 Java Solution

```java
/**
 * Problem: Maximum Depth of Binary Tree (LeetCode 104)
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

    public int maxDepth(TreeNode root) {
        // Base case: empty subtree has depth 0
        if (root == null) {
            return 0;
        }

        // Recursively compute depths of left and right subtrees
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);

        // Current depth is 1 (for root) + max depth of subtrees
        return 1 + Math.max(leftDepth, rightDepth);
    }
}
