### Question
Given a 2-D grid of characters `board` and a string `word`, return `true` if the word is present in the grid, otherwise return `false`.

For the word to be present it must be possible to form it with a path in the board with horizontally or vertically neighboring cells. The same cell may not be used more than once in a word.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/7c1fcf82-71c8-4750-3ddd-4ab6a666a500/public)

```java
Input: 
board = [
  ["A","B","C","D"],
  ["S","A","A","T"],
  ["A","C","A","E"]
],
word = "CAT"

Output: true
```


**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/79721392-44b6-4de7-c571-d3d1640ac100/public)

```java
Input: 
board = [
  ["A","B","C","D"],
  ["S","A","A","T"],
  ["A","C","A","E"]
],
word = "BAT"

Output: false
```

**Constraints:**

- `1 <= board.length, board[i].length <= 5`
- `1 <= word.length <= 10`
- `board` and `word` consists of only lowercase and uppercase English letters.



### Solution

**Word Search Solution**  
**Explanation:**

- The problem requires finding whether a given word exists in a 2D board of characters, where the word can be formed by moving horizontally or vertically through adjacent cells.
- We can solve this problem using a **Depth-First Search (DFS)** approach.
    - For each cell in the board, we start searching for the word by recursively moving in all four directions (up, down, left, right).
    - If a letter in the word matches the current cell, we proceed with the search in the neighboring cells.
    - If we find the word, we return `true`. If we can't form the word, we backtrack and try another path.
    - To avoid revisiting the same cell, we mark the cell as visited and unmark it during backtracking.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {character[][]} board
     * @param {string} word
     * @return {boolean}
     */
    exist(board, word) {
        const rows = board.length;
        const cols = board[0].length;

        // Helper function to perform DFS
        const dfs = (r, c, index) => {
            // If the index is equal to the word length, we have found the word
            if (index === word.length) return true;

            // If the current position is out of bounds or the letter doesn't match
            if (r < 0 || r >= rows || c < 0 || c >= cols || board[r][c] !== word[index]) return false;

            // Save the current character and mark the cell as visited
            const temp = board[r][c];
            board[r][c] = '#'; // Temporarily mark as visited

            // Explore all four directions (up, down, left, right)
            const directions = [[-1, 0], [1, 0], [0, -1], [0, 1]];
            for (const [dr, dc] of directions) {
                if (dfs(r + dr, c + dc, index + 1)) return true;
            }

            // Backtrack: unmark the visited cell
            board[r][c] = temp;
            return false;
        };

        // Iterate through every cell to start the search
        for (let r = 0; r < rows; r++) {
            for (let c = 0; c < cols; c++) {
                if (dfs(r, c, 0)) return true;
            }
        }

        return false; // Word not found
    }
}
```

**Time Complexity:** `O(m * n * 4^k)`

- `m * n` is the number of cells on the board, and `k` is the length of the word. In the worst case, we explore all four directions at each cell, leading to a branching factor of `4^k` where `k` is the length of the word.

**Space Complexity:** `O(m * n)`

- The space complexity is determined by the recursion stack, where the maximum depth of recursion could be the length of the word, but the board size also plays a role in managing visited cells.

**Explanation of Key Steps:**

1. **Depth-First Search (DFS):** We use DFS to try all possible paths from each starting cell, attempting to match each letter of the word.
2. **Backtracking:** After exploring a direction, we backtrack by marking the current cell as unvisited.
3. **Early Termination:** If the word is found at any point, the search terminates early, returning `true`.

---

### Summary of Approach:

|Approach|Time Complexity|Space Complexity|Description|
|---|---|---|---|
|Depth-First Search (DFS) with Backtracking|`O(m * n * 4^k)`|`O(m * n)`|Uses DFS with backtracking to explore all possible word paths.|

This approach efficiently explores all possible paths in the grid and ensures that no cell is revisited during the search. The backtracking mechanism ensures that we can explore alternative paths when necessary.