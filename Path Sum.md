# 🎯 LeetCode 112: Path Sum

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the root of a binary tree and an integer `targetSum`, return `true` if the tree has a root-to-leaf path such that adding up all the values along the path equals `targetSum`.

A **leaf** is a node with no children.

#### Examples

- **Example 1**:
  - **Input**: `root = [5,4,8,11,null,13,4,7,2,null,null,null,1]`, `targetSum = 22`
  - **Output**: `true`
  - **Explanation**: The root-to-leaf path with the target sum is $5 \to 4 \to 11 \to 2$, which sums to $5 + 4 + 11 + 2 = 22$.

- **Example 2**:
  - **Input**: `root = [1,2,3]`, `targetSum = 5`
  - **Output**: `false`
  - **Explanation**: There are two root-to-leaf paths in the tree:
    - $(1 \to 2)$: The sum is $3$.
    - $(1 \to 3)$: The sum is $4$.
    There is no root-to-leaf path with sum = $5$.

- **Example 3**:
  - **Input**: `root = []`, `targetSum = 0`
  - **Output**: `false`
  - **Explanation**: Since the tree is empty, there are no root-to-leaf paths.

#### Constraints

- The number of nodes in the tree is in the range $[0, 5000]$.
- $-1000 \le \text{Node.val} \le 1000$
- $-1000 \le \text{targetSum} \le 1000$

---

### 💡 Intuition & Strategy

1. **Recursive Depth-First Search (DFS)**:
   - Traverse the binary tree starting from the root down to the leaves. 

2. **Target Sum Reduction**:
   - At each step, subtract the current node's value (`root.val`) from the `targetSum`. This transforms the problem into finding if either child branch can fulfill the remaining balance.

3. **Leaf Check Base Condition**:
   - When a node is `null`, return `false`.
   - When we hit a leaf node (both left and right children are `null`), check if the remaining `targetSum` has reached `0`.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Every node in the tree is visited at most once in the worst case.
   - **Space Complexity**: $O(H)$ auxiliary memory
     - Where $H$ is the height of the tree, representing the maximum call stack depth ($O(N)$ for a skewed tree and $O(\log N)$ for a balanced tree).

---

### 💻 Java Solution

```java
/**
 * Problem: Path Sum (LeetCode 112)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(H) where H is the height of the tree
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
    public boolean hasPathSum(TreeNode root, int targetSum) {
        // Base case: if the node is null, there is no path here
        if (root == null) {
            return false;
        }
        
        // Subtract the current node's value from the remaining target sum
        targetSum -= root.val;
        
        // If the current node is a leaf, check if the remaining sum is zero
        if (root.left == null && root.right == null) {
            return targetSum == 0;
        }
        
        // Recursively check the left and right subtrees
        return hasPathSum(root.left, targetSum) || hasPathSum(root.right, targetSum);
    }
}
