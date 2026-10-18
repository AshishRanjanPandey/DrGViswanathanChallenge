# 🎯 LeetCode 199: Binary Tree Right Side View

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

#### Examples

- **Example 1**:
  - **Input**: `root = [1,2,3,null,5,null,4]`
  - **Output**: `[1,3,4]`
  - **Explanation**: 
    - Level 1: `[1]` (Node 1 is visible)
    - Level 2: `[2, 3]` (Node 3 is visible from the right)
    - Level 3: `[5, 4]` (Node 4 is visible from the right)

- **Example 2**:
  - **Input**: `root = [1,2,3,4,null,null,null,5]`
  - **Output**: `[1,3,4,5]`

- **Example 3**:
  - **Input**: `root = [1,null,3]`
  - **Output**: `[1,3]`

- **Example 4**:
  - **Input**: `root = []`
  - **Output**: `[]`

#### Constraints

- The number of nodes in the tree is in the range $[0, 100]$.
- $-100 \le \text{Node.val} \le 100$

---

### 💡 Intuition & Strategy

1. **Level-Order Traversal (BFS)**:
   - To find what is visible from the right side, we can process the binary tree level by level using a Queue (Breadth-First Search).
   - For each level, the nodes are processed sequentially from left to right.

2. **Identifying the Rightmost Node**:
   - As we iterate through the nodes of a given level, the **last node** visited in that loop (i.e., when index `i == levelSize - 1`) is guaranteed to be the rightmost element visible from the right side.
   - We capture this node's value and add it to our result list.

3. **Queue Management**:
   - For every node processed, we push its left child (if any) and its right child (if any) into the queue to prepare for the subsequent level.

4. **Complexity**:
   - **Time Complexity**: $\mathcal{O}(N)$ where $N$ is the total number of nodes in the binary tree. Every node is visited and queued exactly once.
   - **Space Complexity**: $\mathcal{O}(W)$ where $W$ is the maximum width of the binary tree. In the worst-case scenario (a complete binary tree), the bottom level holds $\mathcal{O}(N)$ nodes, resulting in $\mathcal{O}(N)$ space for the queue.

---

### 💻 Java Solution

```java
/**
 * Problem: Binary Tree Right Side View (LeetCode 199)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(W) where W is the max width of the tree
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                
                // The last element of the current level is visible from the right side
                if (i == levelSize - 1) {
                    result.add(currentNode.val);
                }
                
                // Add child nodes to the queue for the next level
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
        }
        
        return result;
    }
}
