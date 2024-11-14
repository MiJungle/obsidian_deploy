### Question
Given the `root` of a _non-empty_ binary tree, return the maximum **path sum** of any _non-empty_ path.

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes has an edge connecting them. A node can _not_ appear in the sequence more than once. The path does _not_ necessarily need to include the root.

The **path sum** of a path is the sum of the node's values in the path.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/9896b041-9021-44c2-ab3e-5cff76adf100/public)

```java
Input: root = [1,2,3]

Output: 6
```


Explanation: The path is 2 -> 1 -> 3 with a sum of 2 + 1 + 3 = 6.

**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/19ce1187-387e-4323-f2c9-1a317ab36200/public)

```java
Input: root = [-15,10,20,null,null,15,5,-5]

Output: 40
```


Explanation: The path is 15 -> 20 -> 5 with a sum of 15 + 20 + 5 = 40.

**Constraints:**

- `1 <= The number of nodes in the tree <= 1000`.
- `-1000 <= Node.val <= 1000`

Here’s a solution for the **Binary Tree Maximum Path Sum** problem on NeetCode in the requested format:

### Solution

**1. Recursive Depth-First Search (DFS) Solution**  
**Explanation:**  
- Use a recursive DFS approach to calculate the maximum path sum that includes each node as the highest node in the path.
- For each node, calculate the maximum gain from its left and right children.
- At each node, compute:
  - **Left gain**: Maximum path sum including the left child, or 0 if negative.
  - **Right gain**: Maximum path sum including the right child, or 0 if negative.
  - **Current path sum**: Sum of the node's value, left gain, and right gain.
- Update the global maximum path sum if the current path sum is higher.
- Return the maximum gain to contribute to the parent’s path (node’s value + max of left or right gain).

**Implementation (JavaScript):**

```javascript
function maxPathSum(root) {
    let maxSum = -Infinity;

    function dfs(node) {
        if (node === null) return 0;

        const leftGain = Math.max(dfs(node.left), 0);  // Ignore negative paths
        const rightGain = Math.max(dfs(node.right), 0);  // Ignore negative paths

        const currentPathSum = node.val + leftGain + rightGain;

        maxSum = Math.max(maxSum, currentPathSum);  // Update max sum if current path is better

        return node.val + Math.max(leftGain, rightGain);  // Return max gain for parent path
    }

    dfs(root);
    return maxSum;
}
```

**Time Complexity:** `O(n)`  
- Each node is visited once.

**Space Complexity:** `O(h)`  
- Stack space due to recursion, where `h` is the height of the tree.

**Advantage:** Efficient for large trees, as each node is processed only once, and it avoids unnecessary calculations.

---

### Summary of Approach

| Approach              | Time Complexity | Space Complexity | Description                          |
|-----------------------|-----------------|------------------|--------------------------------------|
| Recursive DFS         | `O(n)`         | `O(h)`          | Computes the maximum path sum while traversing each node |

The **recursive DFS** approach is optimal for finding the maximum path sum in a binary tree, as it calculates the path sum in a single traversal of the tree.