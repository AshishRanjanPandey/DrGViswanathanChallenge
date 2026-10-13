# 🎯 LeetCode 637: Average of Levels in Binary Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given the `root` of a binary tree, return the average value of the nodes on each level in the form of an array. Answers within $10^{-5}$ of the actual answer will be accepted.

#### Examples

- **Example 1**:
  - **Input**: `root = [3,9,20,null,null,15,7]`
  - **Output**: `[3.00000,14.50000,11.00000]`
  - **Explanation**: 
    - The average of values on level 0 is $3$,
    - on level 1 is $(9 + 20) / 2 = 14.5$,
    - on level 2 is $(15 + 7) / 2 = 11.0$.
    - Hence return `[3.00000, 14.50000, 11.00000]`.

- **Example 2**:
  - **Input**: `root = [3,9,20,15,7]`
  - **Output**: `[3.00000,14.50000,11.00000]`

#### Constraints

- The number of nodes in the tree is in the range $[1, 10^4]$.
- $-2^{31} \le \text{Node.val} \le 2^{31} - 1$

---

### 💡 Intuition & Strategy

1. **Level-Order Traversal (Breadth-First Search / BFS)**:
   - To find the average of each level, we need to process nodes level by level. A Queue data structure is ideal for this since it follows First-In-First-Out (FIFO) order.

2. **Tracking Level Metrics**:
   - For every iteration of the queue, we determine the number of nodes at the current level using `queue.size()`. This exact count dictates how many nodes we process for that specific level.
   - We accumulate the sum of node values (`levelSum`) for the current level while polling elements and pushing their children (`left` and `right`) into the queue.

3. **Computing the Average**:
   - Once all nodes of a level are processed, we divide `levelSum` by the level's node count (`levelSize`) using floating-point division (`double`) to avoid integer truncation.
   - We store this average in our result list before moving to the next level.

4. **Complexity**:
   - **Time Complexity**: $O(N)$, where $N$ is the total number of nodes in the binary tree. Every node is added to and removed from the queue exactly once.
   - **Space Complexity**: $O(M)$, where $M$ is the maximum width of the binary tree. In the worst-case scenario (a complete binary tree), the bottom level holds roughly $N/2$ nodes, making the queue memory footprint linear with respect to the tree's maximum width.

---

### 💻 Java Solution

```java
/**
 * Problem: Average of Levels in Binary Tree (LeetCode 637)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(M) where M is the maximum width of the tree
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

// Definition for a binary tree node.
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            double levelSum = 0;

            for (int i = 0; i < levelSize; i++) {
                TreeNode current = queue.poll();
                levelSum += current.val;

                if (current.left != null) {
                    queue.offer(current.left);
                }
                if (current.right != null) {
                    queue.offer(current.right);
                }
            }

            // Calculate average for the current level and add to result
            result.add(levelSum / levelSize);
        }

        return result;
    }
}
