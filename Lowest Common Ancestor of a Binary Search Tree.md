# 🎯 LeetCode 235: Lowest Common Ancestor of a Binary Search Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself).”

#### Examples

- **Example 1**:
  - **Input**: `root = [6,2,8,0,4,7,9,null,null,3,5]`, `p = 2`, `q = 8`
  - **Output**: `6`
  - **Explanation**: The LCA of nodes `2` and `8` is `6`.

- **Example 2**:
  - **Input**: `root = [6,2,8,0,4,7,9,null,null,3,5]`, `p = 2`, `q = 4`
  - **Output**: `2`
  - **Explanation**: The LCA of nodes `2` and `4` is `2`, since a node can be a descendant of itself according to the LCA definition.

- **Example 3**:
  - **Input**: `root = [2,1]`, `p = 2`, `q = 1`
  - **Output**: `2`

#### Constraints

- The number of nodes in the tree is in the range $[2, 10^5]$.
- $-10^9 \le \text{Node.val} \le 10^9$
- All $\text{Node.val}$ are unique.
- $p \neq q$
- $p$ and $q$ will exist in the BST.

---

### 💡 Intuition & Strategy

1. **Leveraging BST Properties**:
   - In a Binary Search Tree, all values in the left subtree are smaller than the current node, and all values in the right subtree are greater than the current node.
   - This ordering property allows us to navigate directly toward the target nodes without needing a full tree traversal.

2. **Iterative Traversal**:
   - Start from the `root` node and use a pointer `curr`.
   - If both `p.val` and `q.val` are **less than** `curr.val`, the LCA must reside in the **left subtree** (`curr = curr.left`).
   - If both `p.val` and `q.val` are **greater than** `curr.val`, the LCA must reside in the **right subtree** (`curr = curr.right`).
   - If the values split (one is smaller/equal and the other is greater/equal, or one equals `curr.val`), we have found the **split point**, which is our Lowest Common Ancestor.

3. **Complexity**:
   - **Time Complexity**: $O(H)$, where $H$ is the height of the tree. In the worst case (skewed tree), it takes $O(N)$ time. For a balanced BST, it takes $O(\log N)$ time.
   - **Space Complexity**: $O(1)$ auxiliary memory since we use an iterative approach with no recursive stack overhead.

---

### 💻 Java Solution

```java
/**
 * Problem: Lowest Common Ancestor of a Binary Search Tree (LeetCode 235)
 * Language: Java 17
 * Time Complexity: O(H) where H is the height of the tree
 * Space Complexity: O(1) iterative space
 * Challenge: Dr. G. Vishwanathan Challenge
 */

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode curr = root;
        
        while (curr != null) {
            // If both p and q are smaller than curr, go to the left subtree
            if (p.val < curr.val && q.val < curr.val) {
                curr = curr.left;
            }
            // If both p and q are greater than curr, go to the right subtree
            else if (p.val > curr.val && q.val > curr.val) {
                curr = curr.right;
            }
            // Otherwise, we have found the split point (LCA)
            else {
                return curr;
            }
        }
        
        return null;
    }
}
