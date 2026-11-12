# 🎯 LeetCode 124: Binary Tree Maximum Path Sum

> **Submission for**: #DrGVishwanathanChallenge

### 📝 Problem Description

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return *the maximum path sum of any non-empty path*.

#### Examples

* **Example 1**:

  * **Input**: `root = [1,2,3]`

  * **Output**: `6`

  * **Explanation**: The optimal path is `2 -> 1 -> 3` with a path sum of $2 + 1 + 3 = 6$.

* **Example 2**:

  * **Input**: `root = [-10,9,20,null,null,15,7]`

  * **Output**: `42`

  * **Explanation**: The optimal path is `15 -> 20 -> 7` with a path sum of $15 + 20 + 7 = 42$.

#### Constraints

* The number of nodes in the tree is in the range $[1, 3 \cdot 10^4]$.

* $-1000 \le \text{Node.val} \le 1000$

### 💡 Intuition & Strategy

1. **Post-Order Depth-First Search (DFS)**:

   * To calculate the maximum path sum that passes through a given node, we first need to know the maximum contributions (gains) from its left and right subtrees.

   * Using a post-order recursive traversal (Left $\rightarrow$ Right $\rightarrow$ Node), we process the tree bottom-up.

2. **Discarding Negative Contributions**:

   * If a subtree returns a path sum that is negative, adding it to our path will only decrease the overall sum.

   * Therefore, we clamp negative branch sums to $0$ using $\max(0, \text{branchGain})$.

3. **Distinguishing Node Peak vs. Parent Extension**:

   * **Node as Path Inflexion Point**: A path can turn around at the current node, connecting both left and right subtrees. The path sum at this local highest point is:
     

     $$
     \text{currentPathSum} = \text{node.val} + \text{leftGain} + \text{rightGain}
     $$

     
     We update our global maximum (`maxSum`) with this value.

   * **Extending to Parent**: A single valid path cannot split twice. When returning a value back up to the parent node, we can only pick **one** sub-branch (the larger one). Thus, the function returns:
     

     $$
     \text{returnGain} = \text{node.val} + \max(\text{leftGain}, \text{rightGain})
     $$

4. **Complexity**:

   * **Time Complexity**: $O(N)$

     * Every node in the binary tree is visited exactly once during the post-order traversal.

   * **Space Complexity**: $O(H)$ auxiliary stack space

     * Where $H$ is the height of the tree. In the worst case (a completely skewed tree), space is $O(N)$, whereas for a balanced tree, space is $O(\log N)$.

### 💻 Java Solution

```
/**
 * Problem: Binary Tree Maximum Path Sum (LeetCode 124)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(H) recursion stack space
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
    private int maxSum;

    public int maxPathSum(TreeNode root) {
        maxSum = Integer.MIN_VALUE;
        maxGain(root);
        return maxSum;
    }

    private int maxGain(TreeNode node) {
        if (node == null) {
            return 0;
        }

        // Recursively calculate the maximum path sum of left and right subtrees.
        // Ignore negative path sums by capping at 0.
        int leftGain = Math.max(maxGain(node.left), 0);
        int rightGain = Math.max(maxGain(node.right), 0);

        // Compute the path sum where the current node serves as the apex (highest point)
        int currentPathSum = node.val + leftGain + rightGain;

        // Update global maximum path sum
        maxSum = Math.max(maxSum, currentPathSum);

        // Return the maximum single branch sum extended to the parent node
        return node.val + Math.max(leftGain, rightGain);
    }
}

```