# 🎯 LeetCode 212: Word Search II

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given an `m x n` `board` of characters and a list of strings `words`, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

#### Examples

- **Example 1**:
  - **Input**: `board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]]`, `words = ["oath","pea","eat","rain"]`
  - **Output**: `["eat","oath"]`

- **Example 2**:
  - **Input**: `board = [["a","b"],["c","d"]]`, `words = ["abcb"]`
  - **Output**: `[]`

#### Constraints

- $m == \text{board.length}$
- $n == \text{board[i].length}$
- $1 \le m, n \le 12$
- `board[i][j]` is a lowercase English letter.
- $1 \le \text{words.length} \le 3 \times 10^4$
- $1 \le \text{words[i].length} \le 10$
- `words[i]` consists of lowercase English letters.
- All the strings of `words` are unique.

---

### 💡 Intuition & Strategy

1. **Trie (Prefix Tree) Construction**:
   - Instead of searching the board for each word individually (which would be too slow), we build a Trie out of all the target `words`. 
   - This allows us to search the board for all words simultaneously and efficiently check if the current path forms a valid prefix.

2. **DFS with Backtracking on the Board**:
   - Iterate through every cell on the board, treating each as a potential starting point for a word.
   - Perform a Depth-First Search (DFS) to explore all valid neighboring cells (up, down, left, right).
   - To ensure a cell is only used once per word, temporarily mark it as visited (e.g., by changing its character to `#`), and **backtrack** by restoring its original character once the DFS for that path completes.

3. **Simultaneous Traversal & Early Pruning**:
   - As we move through the board, we traverse down the Trie. 
   - **Pruning**: If the current board character doesn't match any child node in the Trie, we immediately halt exploration along that path.
   - **Deduplication**: Once a complete word is found at a Trie node, we add it to our results and remove it from the Trie (by setting the node's word to `null`) to prevent duplicate entries if the word can be formed through multiple paths.

4. **Complexity**:
   - **Time Complexity**: $O(M \times N \times 3^{L-1})$
     - $M$ and $N$ are the dimensions of the board, so there are $M \times N$ starting cells.
     - $L$ is the maximum length of a word. For each starting cell, we explore up to $3$ directions (since we don't go back to the cell we just came from), up to a depth of $L$.
   - **Space Complexity**: $O(\sum L)$ auxiliary memory
     - The Trie requires space proportional to the sum of the lengths of all words.
     - The DFS recursion stack requires $O(L)$ space.

---

### 💻 Java Solution

```java
/**
 * Problem: Word Search II (LeetCode 212)
 * Language: Java 17
 * Time Complexity: O(M * N * 3^(L-1))
 * Space Complexity: O(Sum of word lengths) for Trie + O(L) for call stack
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.List;

class Solution {
    // Define the TrieNode structure
    class TrieNode {
        TrieNode[] children = new TrieNode[26];
        String word; // Stores the word at the leaf node, null otherwise
    }
    
    public List<String> findWords(char[][] board, String[] words) {
        List<String> res = new ArrayList<>();
        TrieNode root = buildTrie(words);
        
        // Start DFS from every cell on the board
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                dfs(board, i, j, root, res);
            }
        }
        
        return res;
    }
    
    private void dfs(char[][] board, int i, int j, TrieNode p, List<String> res) {
        char c = board[i][j];
        
        // Return if out of bounds, already visited, or prefix doesn't exist in Trie
        if (c == '#' || p.children[c - 'a'] == null) return;
        
        p = p.children[c - 'a'];
        
        // If we found a complete word in the Trie
        if (p.word != null) {
            res.add(p.word);
            p.word = null; // Set to null to avoid duplicate entries in the result
        }
        
        // Mark current cell as visited
        board[i][j] = '#';
        
        // Explore all 4 adjacent directions
        if (i > 0) dfs(board, i - 1, j, p, res); // Up
        if (j > 0) dfs(board, i, j - 1, p, res); // Left
        if (i < board.length - 1) dfs(board, i + 1, j, p, res); // Down
        if (j < board[0].length - 1) dfs(board, i, j + 1, p, res); // Right
        
        // Backtrack: restore the cell's original character
        board[i][j] = c;
    }
    
    private TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        for (String w : words) {
            TrieNode p = root;
            for (char c : w.toCharArray()) {
                int i = c - 'a';
                if (p.children[i] == null) {
                    p.children[i] = new TrieNode();
                }
                p = p.children[i];
            }
            p.word = w; // Store the full word at the end node
        }
        return root;
    }
}
```