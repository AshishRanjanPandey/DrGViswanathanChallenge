# 🎯 LeetCode 98: Validate Binary Search Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the `root` of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:
- The left subtree of a node contains only nodes with keys **strictly less than** the node's key.
- The right subtree of a node contains only nodes with keys **strictly greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

#### Examples

- **Example 1**:
  - **Input**: `root = [2,1,3]`
  - **Output**: `true`

- **Example 2**:
  - **Input**: `root = [5,1,4,null,null,3,6]`
  - **Output**: `false`
  - **Explanation**: The root node's value is $5$ but its right child's value is $4$, which violates the BST property because $4$ is not greater than $5$.

#### Constraints

- The number of nodes in the tree is in the range $[1, 10^4]$.
- $-2^{31} \le \text{Node.val} \le 2^{31} - 1$

---

### 💡 Intuition & Strategy

1. **Range Boundary Tracking ($O(N)$ approach)**:
   - A common pitfall is checking only whether a node's immediate children satisfy the BST condition relative to the parent. However, a node deep in the right subtree of a left child must still be less than the root.
   - To correctly validate this, we maintain valid boundaries (`low` and `high`) for every node as we traverse downward.

2. **Recursive Validation Logic**:
   - For any given node, its value must satisfy: `low < node.val < high`.
   - When branching to the left child, the upper bound (`high`) is updated to the current node's value, while the lower bound remains unchanged.
   - When branching to the right child, the lower bound (`low`) is updated to the current node's value, while the upper bound remains unchanged.
   - Using `Integer` (or `Long`) objects allows us to pass `null` for the root, representing unbounded initial limits.

3. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Every node in the tree is visited at most once, where $N$ is the total number of nodes.
   - **Space Complexity**: $O(H)$ where $H$ is the height of the tree
     - Accounts for the recursive call stack storage. In the worst case (skewed tree), it requires $O(N)$, and in the best case (balanced tree), it takes $O(\log N)$.

---

### 💻 Java Solution

```java
/**
 * Problem: Validate Binary Search Tree (LeetCode 98)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(H) recursive call stack
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    public boolean isValidBST(TreeNode root) {
        // Using null bounds initially to represent positive/negative infinity
        return validate(root, null, null);
    }
    
    private boolean validate(TreeNode node, Integer low, Integer high) {
        // An empty tree/subtree is inherently a valid BST
        if (node == null) {
            return true;
        }
        
        // Check if the current node violates the boundary constraints
        if ((low != null && node.val <= low) || (high != null && node.val >= high)) {
            return false;
        }
        
        // Recursively validate left subtree (upper bound updates to node.val)
        // and right subtree (lower bound updates to node.val)
        return validate(node.left, low, node.val) && validate(node.right, node.val, high);
    }
}
```