# 🎯 LeetCode 110: Balanced Binary Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given a binary tree, determine if it is **height-balanced**.

A **height-balanced** binary tree is defined as a binary tree in which the left and right subtrees of *every* node differ in height by no more than $1$.

#### Examples

- **Example 1**:
  - **Input**: `root = [3,9,20,null,null,15,7]`
  - **Output**: `true`
  - **Explanation**: The depth difference between the left and right subtree of every node is at most $1$.

- **Example 2**:
  - **Input**: `root = [1,2,2,3,3,null,null,4,4]`
  - **Output**: `false`
  - **Explanation**: The left subtree has a height of $3$ while the right subtree has a height of $1$, resulting in a difference greater than $1$.

- **Example 3**:
  - **Input**: `root = []`
  - **Output**: `true`
  - **Explanation**: An empty tree is height-balanced by definition.

#### Constraints

- The number of nodes in the tree is in the range $[0, 5000]$.
- $-10^4 \le \text{Node.val} \le 10^4$

---

### 💡 Intuition & Strategy

1. **Bottom-Up Depth-First Search (DFS)**:
   - A naive top-down check computes the height of subtrees repeatedly at every node, resulting in $O(N^2)$ worst-case time complexity.
   - Using a **bottom-up post-order traversal** allows calculating subtree heights from the leaves upward, validating balance at each step in a single pass.

2. **Early Pruning with Sentinel Value**:
   - Return `-1` immediately from helper recursive calls whenever any subtree violates the balance condition ($\vert{}\text{leftHeight} - \text{rightHeight}\vert{} > 1$).
   - If either child subtree returns `-1`, propagate `-1` up the call stack immediately to avoid redundant calculations.

3. **Height Calculation**:
   - For any balanced node, return its actual height: $\max(\text{leftHeight}, \text{rightHeight}) + 1$.
   - The tree is balanced if and only if the final return value of the root call is not `-1`.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Every node in the tree is visited at most once during the post-order traversal.
   - **Space Complexity**: $O(H)$
     - Auxiliary memory on the recursion stack corresponds to tree height $H$ ($O(\log N)$ average for balanced trees, $O(N)$ worst-case for skewed trees).

---

### 💻 Java Solution

```java
/**
 * Problem: Balanced Binary Tree (LeetCode 110)
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

    public boolean isBalanced(TreeNode root) {
        return checkHeight(root) != -1;
    }

    private int checkHeight(TreeNode node) {
        if (node == null) {
            return 0;
        }

        int leftHeight = checkHeight(node.left);
        if (leftHeight == -1) {
            return -1;
        }

        int rightHeight = checkHeight(node.right);
        if (rightHeight == -1) {
            return -1;
        }

        if (Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        }

        return Math.max(leftHeight, rightHeight) + 1;
    }
}
