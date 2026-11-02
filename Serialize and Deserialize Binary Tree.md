# 🎯 LeetCode 297: Serialize and Deserialize Binary Tree

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

#### Examples

- **Example 1**:
  - **Input**: `root = [1,2,3,null,null,4,5]`
  - **Output**: `[1,2,3,null,null,4,5]`

- **Example 2**:
  - **Input**: `root = []`
  - **Output**: `[]`

#### Constraints

- The number of nodes in the tree is in the range $[0, 10^4]$.
- $-1000 \le \text{Node.val} \le 1000$

---

### 💡 Intuition & Strategy

1. **Traversal Choice (Pre-order DFS)**:
   - A Pre-order traversal (Root, Left, Right) is ideal here because the root is processed first. When deserializing, reading the first value immediately gives us the root node, allowing us to naturally construct the left and right subtrees recursively.

2. **Serialization (Encoding)**:
   - Traverse the tree and append each node's value to a `StringBuilder`, followed by a delimiter (like a comma `,`).
   - If a node is `null`, we must record it to maintain the tree's structural integrity. We can use a placeholder character like `"X"` followed by the comma.

3. **Deserialization (Decoding)**:
   - Split the encoded string by the comma delimiter and insert all elements into a `Queue`.
   - Recursively dequeue (poll) elements:
     - If the value is `"X"`, return `null`.
     - Otherwise, instantiate a new `TreeNode` with the parsed integer value.
     - Recursively call the function to build the `left` subtree, then the `right` subtree. The queue ensures elements are processed in the exact order they were serialized.

4. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Both serialization and deserialization visit each node exactly once.
   - **Space Complexity**: $O(N)$
     - Serialization requires $O(N)$ space for the `StringBuilder` and recursion stack. Deserialization requires $O(N)$ space to store the split string in a Queue and for the recursion stack.

---

### 💻 Java Solution

```java
/**
 * Problem: Serialize and Deserialize Binary Tree (LeetCode 297)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(N) for string building, queue, and recursion stack
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        buildString(root, sb);
        return sb.toString();
    }

    private void buildString(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.append("X").append(",");
        } else {
            sb.append(node.val).append(",");
            buildString(node.left, sb);
            buildString(node.right, sb);
        }
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        // Split the string by commas and load it into a queue for sequential processing
        Queue<String> nodes = new LinkedList<>(Arrays.asList(data.split(",")));
        return buildTree(nodes);
    }

    private TreeNode buildTree(Queue<String> nodes) {
        String val = nodes.poll();
        
        // "X" represents a null pointer
        if (val.equals("X")) {
            return null;
        }
        
        // Create the node and recursively build its left and right subtrees
        TreeNode node = new TreeNode(Integer.parseInt(val));
        node.left = buildTree(nodes);
        node.right = buildTree(nodes);
        
        return node;
    }
}

// Definition for a binary tree node.
// class TreeNode {
//     int val;
//     TreeNode left;
//     TreeNode right;
//     TreeNode(int x) { val = x; }
// }
