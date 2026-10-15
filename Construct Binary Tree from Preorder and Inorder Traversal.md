# 🎯 LeetCode 105: Construct Binary Tree from Preorder and Inorder Traversal

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return the binary tree.

#### Examples

- **Example 1**:
  - **Input**: `preorder = [3,9,20,15,7]`, `inorder = [9,3,15,20,7]`
  - **Output**: `[3,9,20,null,null,15,7]`

- **Example 2**:
  - **Input**: `preorder = [-1]`, `inorder = [-1]`
  - **Output**: `[-1]`

#### Constraints

- $1 \le \text{preorder.length} \le 3000$
- $\text{inorder.length} == \text{preorder.length}$
- $-3000 \le \text{preorder[i]}, \text{inorder[i]} \le 3000$
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is guaranteed to be the preorder traversal of the tree.
- `inorder` is guaranteed to be the inorder traversal of the tree.

---

### 💡 Intuition & Strategy

1. **Preorder Property**: 
   - The first element in a preorder traversal array (`preorder[0]`) is always the root of the current (sub)tree.

2. **Inorder Property**: 
   - In an inorder traversal array, the root element divides the array into two distinct parts: elements to its left belong to the left subtree, and elements to its right belong to the right subtree.

3. **Hash Map Optimization**: 
   - Instead of scanning the `inorder` array linearly to find the root's position every time (which would result in an $O(N^2)$ time complexity), we pre-populate a `HashMap` to store each value and its corresponding index in the `inorder` array. This allows $O(1)$ index lookups.

4. **Recursive Tree Building**: 
   - We use a helper function with pointers to track boundaries (`left` and `right`) in the `inorder` array and a global/instance pointer for the `preorderIndex`. We recursively construct the left and right subtrees.

5. **Complexity**:
   - **Time Complexity**: $O(N)$
     - Every node is visited once, and map lookups take $O(1)$ time.
   - **Space Complexity**: $O(N)$
     - Required for storing the `HashMap` and the recursion call stack (which takes $O(N)$ in the worst-case scenario of a skewed tree).

---

### 💻 Java Solution

```java
/**
 * Problem: Construct Binary Tree from Preorder and Inorder Traversal (LeetCode 105)
 * Language: Java 17
 * Time Complexity: O(N)
 * Space Complexity: O(N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.HashMap;
import java.util.Map;

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
    private int preorderIndex;
    private Map<Integer, Integer> inorderIndexMap;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        preorderIndex = 0;
        inorderIndexMap = new HashMap<>();
        
        // Populate the map for quick lookup of inorder indices
        for (int i = 0; i < inorder.length; i++) {
            inorderIndexMap.put(inorder[i], i);
        }
        
        return arrayToTree(preorder, 0, preorder.length - 1);
    }
    
    private TreeNode arrayToTree(int[] preorder, int left, int right) {
        // If there are no elements to construct the subtree, return null
        if (left > right) {
            return null;
        }
        
        // Select the current root value from preorder using preorderIndex and increment it
        int rootValue = preorder[preorderIndex++];
        TreeNode root = new TreeNode(rootValue);
        
        // Build left and right subtrees based on inorder indices
        int inorderIndex = inorderIndexMap.get(rootValue);
        
        root.left = arrayToTree(preorder, left, inorderIndex - 1);
        root.right = arrayToTree(preorder, inorderIndex + 1, right);
        
        return root;
    }
}
