# 🎯 LeetCode 51: N-Queens

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *all distinct solutions to the **n-queens puzzle***. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

#### Examples

- **Example 1**:
  - **Input**: `n = 4`
  - **Output**: `[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]`
  - **Explanation**: There exist two distinct solutions to the 4-queens puzzle as shown below:
    - Solution 1: `[".Q..", "...Q", "Q...", "..Q."]`
    - Solution 2: `["..Q.", "Q...", "...Q", ".Q.."]`

- **Example 2**:
  - **Input**: `n = 1`
  - **Output**: `[["Q"]]`
  - **Explanation**: On a $1 \times 1$ board, placing a single queen satisfies the condition.

#### Constraints

- $1 \le n \le 9$

---

### 💡 Intuition & Strategy

1. **Row-by-Row Backtracking**:
   - Because no two queens can occupy the same row, we can place exactly one queen per row sequentially from `row = 0` to `n - 1`.
   - At each row, we iterate through every column `col` from `0` to `n - 1` and check if placing a queen at `(row, col)` is valid.

2. **$O(1)$ Fast Conflict Lookups**:
   - Checking standard diagonals and columns by traversing the board takes $O(N)$ time per placement. Instead, we use three boolean array trackers for $O(1)$ lookup time:
     - `cols[col]`: Tracks whether column `col` is occupied.
     - `diag1[row - col + n]`: Tracks the main (top-left to bottom-right) diagonal. Along this diagonal, `row - col` is constant. Offset `+ n` prevents negative array indexing.
     - `diag2[row + col]`: Tracks the anti-diagonal (top-right to bottom-left). Along this diagonal, `row + col` is constant.

3. **State Placement & Backtracking**:
   - If `(row, col)` is safe, mark column and diagonal arrays as `true` and set `board[row][col] = 'Q'`.
   - Recursively call `backtrack(row + 1, ...)` to place queens in subsequent rows.
   - After exploring that path, **undo the choice** (unmark arrays to `false` and restore `board[row][col] = '.'`) to attempt alternative placements for the current row.

4. **Complexity**:
   - **Time Complexity**: $O(N!)$
     - The first row has $N$ placement choices, the second row has at most $N - 2$ valid choices, the third row has at most $N - 4$, resulting in an overall search tree bound by $O(N!)$.
   - **Space Complexity**: $O(N^2)$
     - Requires $O(N^2)$ memory to maintain the $N \times N$ character board grid, plus $O(N)$ memory for the recursion call stack and lookup tracking arrays.

---

### 💻 Java Solution

```java
/**
 * Problem: N-Queens (LeetCode 51)
 * Language: Java 17
 * Time Complexity: O(N!)
 * Space Complexity: O(N^2) auxiliary (for board layout & recursion stack)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> results = new ArrayList<>();
        char[][] board = new char[n][n];
        for (char[] row : board) {
            Arrays.fill(row, '.');
        }

        boolean[] cols = new boolean[n];
        boolean[] diag1 = new boolean[2 * n]; // Main diagonals (row - col + n)
        boolean[] diag2 = new boolean[2 * n]; // Anti-diagonals (row + col)

        backtrack(0, n, board, cols, diag1, diag2, results);
        return results;
    }

    private void backtrack(int row, int n, char[][] board, boolean[] cols,
                           boolean[] diag1, boolean[] diag2, List<List<String>> results) {
        // Base case: successfully placed queens in all rows
        if (row == n) {
            results.add(constructBoard(board));
            return;
        }

        for (int col = 0; col < n; col++) {
            int d1 = row - col + n;
            int d2 = row + col;

            // Skip if column or diagonals are already occupied
            if (cols[col] || diag1[d1] || diag2[d2]) {
                continue;
            }

            // Place queen and update lookup state
            board[row][col] = 'Q';
            cols[col] = true;
            diag1[d1] = true;
            diag2[d2] = true;

            // Recurse to the next row
            backtrack(row + 1, n, board, cols, diag1, diag2, results);

            // Backtrack: remove queen and restore lookup state
            board[row][col] = '.';
            cols[col] = false;
            diag1[d1] = false;
            diag2[d2] = false;
        }
    }

    private List<String> constructBoard(char[][] board) {
        List<String> path = new ArrayList<>();
        for (char[] row : board) {
            path.add(new String(row));
        }
        return path;
    }
}
```