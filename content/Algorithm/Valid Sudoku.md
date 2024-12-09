### Question
Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

**Example 1:**

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**Input:** board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
**Output:** true

**Example 2:**

**Input:** board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
**Output:** false
**Explanation:** Same as Example 1, except with the **5** in the top left corner being modified to **8**. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

**Constraints:**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit `1-9` or `'.'`.


### Solution
To validate a Sudoku board, you need to check if each row, column, and 3x3 subgrid contains only unique numbers (1 through 9), ignoring empty cells (denoted by `'.'`). Here are three approaches, progressively optimized in complexity and clarity.

---

### 1. **Brute Force Solution: Check All Rows, Columns, and Grids Independently**

**Explanation**:  
- Traverse the board three times: once for rows, once for columns, and once for each 3x3 grid.
- For each row, column, and grid, check if the numbers (ignoring `'.'`) are unique by keeping track in a set.
  
**Implementation (JavaScript)**:

```javascript
function isValidSudoku(board) {
    const checkUnique = (arr) => {
        const seen = new Set();
        for (const num of arr) {
            if (num !== '.' && seen.has(num)) return false;
            seen.add(num);
        }
        return true;
    };

    // Check rows and columns
    for (let i = 0; i < 9; i++) {
        const row = board[i];
        const col = board.map(row => row[i]);
        if (!checkUnique(row) || !checkUnique(col)) return false;
    }

    // Check 3x3 sub-grids
    for (let i = 0; i < 9; i += 3) {
        for (let j = 0; j < 9; j += 3) {
            const grid = [];
            for (let x = i; x < i + 3; x++) {
                for (let y = j; y < j + 3; y++) {
                    grid.push(board[x][y]);
                }
            }
            if (!checkUnique(grid)) return false;
        }
    }
    return true;
}
```


```js
const isValidSudokuBruteForce = (board) => {
  // Helper function to check if a set of numbers is valid
  const isValidGroup = (group) => {
    const seen = new Set();
    for (const value of group) {
      if (value === ".") continue;
      if (seen.has(value)) return false;
      seen.add(value);
    }
    return true;
  };

  // Check all rows
  for (let row = 0; row < 9; row++) {
    const rowValues = board[row];
    if (!isValidGroup(rowValues)) return false;
  }

  // Check all columns
  for (let col = 0; col < 9; col++) {
    const colValues = [];
    for (let row = 0; row < 9; row++) {
      colValues.push(board[row][col]);
    }
    if (!isValidGroup(colValues)) return false;
  }

  // Check all 3x3 sub-boxes
  for (let boxRow = 0; boxRow < 3; boxRow++) {
    for (let boxCol = 0; boxCol < 3; boxCol++) {
      const boxValues = [];
      for (let row = 0; row < 3; row++) {
        for (let col = 0; col < 3; col++) {
          boxValues.push(board[boxRow * 3 + row][boxCol * 3 + col]);
        }
      }
      if (!isValidGroup(boxValues)) return false;
    }
  }

  return true;
};

// Example usage
const board = [
  ["5", "3", ".", ".", "7", ".", ".", ".", "."],
  ["6", ".", ".", "1", "9", "5", ".", ".", "."],
  [".", "9", "8", ".", ".", ".", ".", "6", "."],
  ["8", ".", ".", ".", "6", ".", ".", ".", "3"],
  ["4", ".", ".", "8", ".", "3", ".", ".", "1"],
  ["7", ".", ".", ".", "2", ".", ".", ".", "6"],
  [".", "6", ".", ".", ".", ".", "2", "8", "."],
  [".", ".", ".", "4", "1", "9", ".", ".", "5"],
  [".", ".", ".", ".", "8", ".", ".", "7", "9"],
];

console.log(isValidSudokuBruteForce(board)); // Output: true

```

**Time Complexity**:  
- `O(9^2)`: Checking each row, column, and grid once.

**Space Complexity**:  
- `O(9)`: Using a set for each row, column, or grid.

**Drawback**: This approach does redundant checks and could be optimized by combining row, column, and grid checks in a single pass.

---

### 2. **Optimized Single-Pass Solution with Sets**

**Explanation**:  
- Use a single traversal to check rows, columns, and grids concurrently.
- Maintain three sets: one for rows, one for columns, and one for each 3x3 grid, indexed by a unique identifier.

**Implementation (JavaScript)**:

```javascript
function isValidSudoku(board) {
    const rows = Array.from({ length: 9 }, () => new Set());
    const cols = Array.from({ length: 9 }, () => new Set());
    const grids = Array.from({ length: 9 }, () => new Set());

    for (let i = 0; i < 9; i++) {
        for (let j = 0; j < 9; j++) {
            const num = board[i][j];
            if (num === '.') continue;

            const gridIndex = Math.floor(i / 3) * 3 + Math.floor(j / 3);

            if (rows[i].has(num) || cols[j].has(num) || grids[gridIndex].has(num)) {
                return false;
            }

            rows[i].add(num);
            cols[j].add(num);
            grids[gridIndex].add(num);
        }
    }
    return true;
}
```

**Time Complexity**:  
- `O(9^2)`: Each cell is processed once.

**Space Complexity**:  
- `O(9^2)`: Storing sets for rows, columns, and grids.

**Advantage**: Efficiently handles validation with a single pass, reducing the need for redundant checks.

---

### 3. **Array-Based Solution for Reduced Space Complexity**

**Explanation**:  
- Instead of sets, use arrays where each index represents a number (1–9) and increment if the number is encountered in a row, column, or grid.
- Reset the arrays for each row, column, or grid to track unique entries.

**Implementation (JavaScript)**:

```javascript
function isValidSudoku(board) {
    for (let i = 0; i < 9; i++) {
        const rowCheck = Array(9).fill(0);
        const colCheck = Array(9).fill(0);
        const gridCheck = Array(9).fill(0);

        for (let j = 0; j < 9; j++) {
            // Row check
            if (board[i][j] !== '.') {
                const num = board[i][j] - '1';
                if (rowCheck[num]++) return false;
            }

            // Column check
            if (board[j][i] !== '.') {
                const num = board[j][i] - '1';
                if (colCheck[num]++) return false;
            }

            // 3x3 grid check
            const rowIdx = 3 * Math.floor(i / 3) + Math.floor(j / 3);
            const colIdx = 3 * (i % 3) + (j % 3);
            if (board[rowIdx][colIdx] !== '.') {
                const num = board[rowIdx][colIdx] - '1';
                if (gridCheck[num]++) return false;
            }
        }
    }
    return true;
}
```

**Time Complexity**:  
- `O(9^2)`: Each cell is processed once.

**Space Complexity**:  
- `O(9)`: Space for row, column, and grid arrays, reused each time.

**Advantage**: Reduces space usage by using fixed arrays instead of sets, making it memory efficient.

---

### Summary of Approaches:

| Approach                       | Time Complexity | Space Complexity | Description                                 |
|--------------------------------|-----------------|------------------|---------------------------------------------|
| Brute Force Solution           | `O(9^2)`       | `O(9)`          | Checks rows, columns, and grids separately. |
| Optimized Single-Pass Solution | `O(9^2)`       | `O(9^2)`        | Validates in one pass using multiple sets.  |
| Array-Based Solution           | `O(9^2)`       | `O(9)`          | Uses arrays for reduced memory usage.       |

**Best Solution**: The **Optimized Single-Pass Solution** is clear, efficient, and concise, although the **Array-Based Solution** offers a lower memory footprint if space is a concern.