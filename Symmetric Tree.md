# 🎯 LeetCode 101: Symmetric Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

#### Examples

- **Example 1**:
  - **Input**: `root = [1,2,2,3,4,4,3]`
  - **Output**: `true`
  - **Explanation**: The tree is symmetric around its center.

- **Example 2**:
  - **Input**: `root = [1,2,2,null,3,null,3]`
  - **Output**: `false`
  - **Explanation**: The tree is not symmetric because of the asymmetric positioning of the nodes on the right/left branches.

#### Constraints

- The number of nodes in the tree is in the range $[1, 1000]$.
- $-100 \le \text{Node.val} \le 100$

---

### 💡 Intuition & Strategy

1. **Recursive Reflection**:
   - A binary tree is symmetric if its left subtree is a mirror reflection of its right subtree.
   - For two subtrees to be mirrors of each other:
     - Their root values must be equal.
     - The right subtree of the first tree must be a mirror reflection of the left subtree of the second tree, and vice-versa.

2. **State Validation**:
   - Write a helper function, e.g., `isMirror(TreeNode t1, TreeNode t2)`, that evaluates:
     - If both nodes are `null`, they match (return `true`).
     - If only one is `null` or values don't match, they don't match (return `false`).
     - Recursively check `isMirror(t1.left, t2.right)` and `isMirror(t1.right, t2.left)`.

3. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Every node in the tree is visited once during the recursive check.
   - **Space Complexity**: $O(N)$ in the worst-case scenario (for a completely skewed tree) to maintain the recursive call stack, and $O(\log N)$ for a completely balanced tree.

---

### 💻 Java Solution

```java
/**
 * Problem: Symmetric Tree (LeetCode 101)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(N)
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
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        return isMirror(root.left, root.right);
    }
    
    private boolean isMirror(TreeNode t1, TreeNode t2) {
        // If both subtrees reach the end, they are symmetric
        if (t1 == null && t2 == null) {
            return true;
        }
        // If only one is null, they are not symmetric
        if (t1 == null || t2 == null) {
            return false;
        }
        
        // Values must match, and outer/inner children must be mirrors of each other
        return (t1.val == t2.val) 
            && isMirror(t1.left, t2.right) 
            && isMirror(t1.right, t2.left);
    }
}
