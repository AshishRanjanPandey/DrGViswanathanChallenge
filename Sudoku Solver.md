# 🎯 LeetCode 37: Sudoku Solver

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:
1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the digits `1-9` must occur exactly once in each of the 9 $3 \times 3$ sub-boxes of the grid.

The `'.'` character indicates empty cells.

#### Examples

- **Example 1**:
  - **Input**:
    ```text
    board = [
      ["5","3",".",".","7",".",".",".","."],
      ["6",".",".","1","9","5",".",".","."],
      [".","9","8",".",".",".",".","6","."],
      ["8",".",".",".","6",".",".",".","3"],
      ["4",".",".","8",".","3",".",".","1"],
      ["7",".",".",".","2",".",".",".","6"],
      [".","6",".",".",".",".","2","8","."],
      [".",".",".","4","1","9",".",".","5"],
      [".",".",".",".","8",".",".","7","9"]
    ]
    ```
  - **Output**:
    ```text
    [
      ["5","3","4","6","7","8","9","1","2"],
      ["6","7","2","1","9","5","3","4","8"],
      ["1","9","8","3","4","2","5","6","7"],
      ["8","5","9","7","6","1","4","2","3"],
      ["4","2","6","8","5","3","7","9","1"],
      ["7","1","3","9","2","4","8","5","6"],
      ["9","6","1","5","3","7","2","8","4"],
      ["2","8","7","4","1","9","6","3","5"],
      ["3","4","5","2","8","6","1","7","9"]
    ]
    ```
  - **Explanation**: The input board has a single valid Sudoku solution, shown in the output above.

#### Constraints

- $\text{board.length} == 9$
- $\text{board}[i].\text{length} == 9$
- `board[i][j]` is a digit (`'1'-'9'`) or `'.'`.
- It is guaranteed that the input board has **only one** solution.

---

### 💡 Intuition & Strategy

1. **$O(1)$ Constraint Validation via State Tracking**:
   - Scanning the entire row, column, and $3 \times 3$ sub-box for every candidate digit adds unnecessary overhead.
   - Instead, maintain three $9 \times 10$ boolean arrays (`rows`, `cols`, and `boxes`) to record which digits `1-9` are currently placed. Pre-populate these arrays with the initial board state before starting the search.

2. **Mapping Cells to $3 \times 3$ Sub-Boxes**:
   - Any cell at coordinates $(r, c)$ maps to one of the 9 sub-boxes (indexed `0` to `8`) using the formula:
     $$\text{boxIdx} = \lfloor r / 3 \rfloor \times 3 + \lfloor c / 3 \rfloor$$

3. **Depth-First Search & Backtracking**:
   - Traverse the board cell by cell from `(0, 0)` moving left-to-right, top-to-bottom.
   - If a cell is already filled, skip directly to the next cell.
   - If a cell is empty (`'.'`), iterate through digits `1` to `9`:
     - If the digit is unused in the current row, column, and sub-box, place it on the board and mark it as used in all three tracking arrays.
     - Recurse to the next cell. If the recursive call returns `true`, propagate `true` immediately since the unique solution has been found.
     - Otherwise, **backtrack**: reset the cell to `'.'` and unmark the digit in the tracking arrays before trying the next candidate.
   - When the row index reaches `9`, all cells have been validly filled and the puzzle is solved.

4. **Complexity**:
   - **Time Complexity**: $O(9^E)$, where $E$ is the number of empty cells (at most $81$).
     - Constraint pruning eliminates invalid branches early, making the search extremely fast in practice. Because the board dimensions are fixed at $9 \times 9$, the theoretical upper bound is constant.
   - **Space Complexity**: $O(1)$
     - The three boolean tracking arrays ($9 \times 10$ each) and the maximum recursion stack depth (at most $81$ frames) use fixed memory independent of the input.

---

### 💻 Java Solution

```java
/**
 * Problem: Sudoku Solver (LeetCode 37)
 * Language: Java 17
 * Time Complexity: O(9^E) where E is the number of empty cells (bounded constant for 9x9)
 * Space Complexity: O(1) fixed auxiliary memory & recursion stack
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class Solution {
    private boolean[][] rows = new boolean[9][10];
    private boolean[][] cols = new boolean[9][10];
    private boolean[][] boxes = new boolean[9][10];

    public void solveSudoku(char[][] board) {
        // 1. Pre-populate tracking arrays with initial board values
        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                if (board[r][c] != '.') {
                    int num = board[r][c] - '0';
                    int boxIdx = (r / 3) * 3 + (c / 3);
                    rows[r][num] = true;
                    cols[c][num] = true;
                    boxes[boxIdx][num] = true;
                }
            }
        }

        // 2. Start backtracking from the top-left cell
        backtrack(board, 0, 0);
    }

    private boolean backtrack(char[][] board, int r, int c) {
        // Base case: reached past the last row, puzzle is solved
        if (r == 9) {
            return true;
        }

        // Move to the next row if we've reached the end of the current row
        if (c == 9) {
            return backtrack(board, r + 1, 0);
        }

        // Skip pre-filled cells
        if (board[r][c] != '.') {
            return backtrack(board, r, c + 1);
        }

        int boxIdx = (r / 3) * 3 + (c / 3);

        // Try placing digits 1 through 9
        for (int num = 1; num <= 9; num++) {
            if (!rows[r][num] && !cols[c][num] && !boxes[boxIdx][num]) {
                // Place the digit
                board[r][c] = (char) (num + '0');
                rows[r][num] = true;
                cols[c][num] = true;
                boxes[boxIdx][num] = true;

                // Recurse to the next cell
                if (backtrack(board, r, c + 1)) {
                    return true;
                }

                // Backtrack: undo the placement
                board[r][c] = '.';
                rows[r][num] = false;
                cols[c][num] = false;
                boxes[boxIdx][num] = false;
            }
        }

        // No valid digit found for this cell
        return false;
    }
}
```
