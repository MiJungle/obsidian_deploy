### Question
You are given an array of integers `candidates`, which may contain duplicates, and a target integer `target`. Your task is to return a list of all **unique combinations** of `candidates` where the chosen numbers sum to `target`.

Each element from `candidates` may be chosen **at most once** within a combination. The solution set must not contain duplicate combinations.

You may return the combinations in **any order** and the order of the numbers in each combination can be in **any order**.

**Example 1:**

```java
Input: candidates = [9,2,2,4,6,1,5], target = 8

Output: [
  [1,2,5],
  [2,2,4],
  [2,6]
]
```

**Example 2:**

```java
Input: candidates = [1,2,3,4,5], target = 7

Output: [
  [1,2,4],
  [2,5],
  [3,4]
]
```

**Constraints:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`


### ### Solution

**Combination Sum II Solution**  
**Explanation:**

- The goal is to find all unique combinations in a list of numbers that sum to a given target. Each number in the list may only be used once in each combination.
- Since the input may contain duplicates, we need to ensure that combinations are unique.
- We can use a recursive Depth-First Search (DFS) approach to explore all possible combinations.
- In each step of the recursion:
    1. Include the current number and reduce the target.
    2. Skip duplicate numbers to avoid generating the same combination more than once.
    3. If the target becomes zero, add the current combination to the result.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {number[]} candidates
     * @param {number} target
     * @return {number[][]}
     */
    combinationSum2(candidates, target) {
        candidates.sort((a, b) => a - b);  // Sort the numbers to easily skip duplicates
        const result = [];
        this.dfs(candidates, target, 0, [], result);
        return result;
    }

    /**
     * @param {number[]} candidates
     * @param {number} target
     * @param {number} start
     * @param {number[]} currentCombination
     * @param {number[][]} result
     */
    dfs(candidates, target, start, currentCombination, result) {
        if (target === 0) {
            result.push([...currentCombination]);
            return;
        }

        for (let i = start; i < candidates.length; i++) {
            // Skip duplicates
            if (i > start && candidates[i] === candidates[i - 1]) continue;

            // If the number is greater than the target, no need to proceed further
            if (candidates[i] > target) break;

            // Include the current number and move forward
            currentCombination.push(candidates[i]);
            this.dfs(candidates, target - candidates[i], i + 1, currentCombination, result);
            currentCombination.pop();  // Backtrack
        }
    }
}
```

**Time Complexity:** `O(2^n)`

- In the worst case, we explore every possible subset of the input list, which is exponential in nature. However, the pruning from skipping duplicates and early stopping when the target is exceeded helps reduce the number of paths we explore.

**Space Complexity:** `O(n)`

- The space complexity is determined by the depth of the recursion and the size of the current combination, which can be at most `n` in length.

**Explanation of Key Steps:**

1. **Sorting the Candidates:** Sorting helps in easily skipping duplicate numbers by comparing consecutive elements.
2. **Recursive DFS:** We try including each number and explore further, backtracking when we exceed the target.
3. **Pruning the Search:** If a candidate exceeds the remaining target, further exploration is stopped early.

---

### Summary of Approach:

|Approach|Time Complexity|Space Complexity|Description|
|---|---|---|---|
|Recursive DFS with Backtracking|`O(2^n)`|`O(n)`|Uses DFS with backtracking and pruning to explore all combinations.|

This approach ensures we explore all unique combinations while efficiently avoiding duplicates and unfeasible paths.
