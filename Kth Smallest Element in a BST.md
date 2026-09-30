# 🎯 LeetCode 230: Kth Smallest Element in a BST

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the root of a binary search tree, and an integer `k`, return the $k$-th smallest value (1-indexed) of all the values of the nodes in the tree.

#### Examples

- **Example 1**:
  - **Input**: `root = [3,1,4,null,2], k = 1`
  - **Output**: `1`
  - **Explanation**: The 1st smallest element is `1`.

- **Example 2**:
  - **Input**: `root = [5,3,6,2,4,null,null,1], k = 3`
  - **Output**: `3`
  - **Explanation**: The elements in ascending order are `[1, 2, 3, 4, 5, 6]`. The 3rd smallest element is `3`.

#### Constraints

- The number of nodes in the tree is $n$.
- $1 \le k \le n \le 10^4$
- $0 \le \text{Node.val} \le 10^4$

---

### 💡 Intuition & Strategy

1. **BST Inorder Traversal Property**:
   - A Binary Search Tree (BST) has the fundamental property that an **inorder traversal (Left $\rightarrow$ Root $\rightarrow$ Right)** visits nodes in strictly ascending order.

2. **Iterative Stack-Based Early Stopping**:
   - Rather than traversing the entire tree to build a sorted list, we can use an explicit stack to simulate the inorder traversal.
   - As we pop nodes and process them, we decrement $k$. The moment $k$ reaches `0`, we have found our target value and can exit immediately, saving time.

3. **Complexity**:
   - **Time Complexity**: $O(H + k)$, where $H$ is the height of the tree. In the worst case (skewed tree), it is $O(n)$, and in a balanced tree, it is $O(\log n + k)$.
   - **Space Complexity**: $O(H)$ auxiliary memory to maintain the stack frames up to the height of the tree.

4. **Follow-Up Strategy (Handling Frequent Modifications)**:
   - **The Challenge**: If inserts and deletes happen frequently, standard traversals degrade to $O(n)$ per query or update.
   - **The Optimization**: **Augment each node with a `leftCount`** property tracking the total number of nodes in its left subtree. 
   - Using this count, we can determine the rank of any node in $O(H)$ time:
     - If $k == \text{leftCount} + 1$, the current node is the answer.
     - If $k \le \text{leftCount}$, the target is in the left subtree.
     - If $k > \text{leftCount}$, the target is in the right subtree (search for $k - \text{leftCount} - 1$).
   - Pairing this augmented structure with a **self-balancing BST (e.g., AVL or Red-Black Tree)** guarantees that searches, inserts, and deletes all execute in $O(\log n)$ time.

---

### 💻 Java Solution

```java
/**
 * Problem: Kth Smallest Element in a BST (LeetCode 230)
 * Language: Java 17
 * Time Complexity: O(H + k)
 * Space Complexity: O(H) auxiliary stack space
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.Stack;

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
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        
        while (curr != null || !stack.isEmpty()) {
            // Reach the leftmost node of the current node
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            
            // Pop from stack and process the node
            curr = stack.pop();
            k--;
            
            if (k == 0) {
                return curr.val;
            }
            
            // Move to the right subtree
            curr = curr.right;
        }
        
        return -1; // Should not be reached for valid inputs where 1 <= k <= n
    }
}
