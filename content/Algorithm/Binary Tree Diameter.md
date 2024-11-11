### Question
The **diameter** of a binary tree is defined as the **length** of the longest path between _any two nodes within the tree_. The path does not necessarily have to pass through the root.

The **length** of a path between two nodes in a binary tree is the number of edges between the nodes.

Given the root of a binary tree `root`, return the **diameter** of the tree.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/90e1d7a0-4322-4c5d-c59b-dde2bf92bb00/public)

```java
Input: root = [1,null,2,3,4,5]

Output: 3
```

Explanation: 3 is the length of the path `[1,2,3,5]` or `[5,3,2,4]`.

**Example 2:**

```java
Input: root = [1,2,3]

Output: 2
```

**Constraints:**

- `1 <= number of nodes in the tree <= 100`
- `-100 <= Node.val <= 100`


### Solution

**1. Recursive Depth-First Search (DFS) Solution**  
**Explanation:**  
- The diameter of a binary tree is the length of the longest path between any two nodes. This path may or may not pass through the root.
- Using a recursive DFS approach, we calculate:
  1. The depth of the left and right subtrees of each node.
  2. The diameter passing through the node, which is the sum of the left and right subtree depths.
  3. The maximum of these diameters across all nodes.
- We update a `maxDiameter` variable to store the longest path encountered.

**Implementation (JavaScript):**

```javascript
class TreeNode {
    constructor(val = 0, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {
    constructor() {
        this.maxDiameter = 0;
    }

    /**
     * @param {TreeNode} root
     * @return {number}
     */
    diameterOfBinaryTree(root) {
        this.calculateDepth(root);
        return this.maxDiameter;
    }

    calculateDepth(node) {
        if (!node) return 0;

        const leftDepth = this.calculateDepth(node.left);
        const rightDepth = this.calculateDepth(node.right);

        // Update max diameter at this node
        this.maxDiameter = Math.max(this.maxDiameter, leftDepth + rightDepth);

        // Return the depth of the node
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```

**Time Complexity:** `O(n)`  
- We visit each node once to calculate its depth and update the diameter.

**Space Complexity:** `O(h)`  
- Space complexity is `O(h)` due to the recursive call stack, where `h` is the height of the tree.

**Drawback:** This approach may lead to a stack overflow on very deep trees in languages without tail call optimization.

---

**2. Iterative Depth-First Search with Stack Solution**  
**Explanation:**  
- We use an iterative DFS approach with a stack, where each node in the stack is associated with a depth.
- For each node:
  1. We track the depth of its left and right subtrees.
  2. Update the `maxDiameter` as the maximum of the current diameter and the depth of the left plus the right subtree.
- By using a stack, we avoid recursion and manage memory efficiently.

**Implementation (JavaScript):**

```javascript
class TreeNode {
    constructor(val = 0, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {
    /**
     * @param {TreeNode} root
     * @return {number}
     */
    diameterOfBinaryTree(root) {
        if (!root) return 0;

        let maxDiameter = 0;
        const stack = [{ node: root, depth: 0 }];
        const depthMap = new Map();

        while (stack.length > 0) {
            const { node } = stack.pop();

            if (node) {
                if (!depthMap.has(node)) {
                    // Add children to the stack before revisiting this node
                    stack.push({ node, revisit: true });
                    if (node.left) stack.push({ node: node.left });
                    if (node.right) stack.push({ node: node.right });
                } else {
                    // Calculate diameter and depth after both children are processed
                    const leftDepth = depthMap.get(node.left) || 0;
                    const rightDepth = depthMap.get(node.right) || 0;

                    maxDiameter = Math.max(maxDiameter, leftDepth + rightDepth);
                    depthMap.set(node, Math.max(leftDepth, rightDepth) + 1);
                }
            }
        }

        return maxDiameter;
    }
}
```

**Time Complexity:** `O(n)`  
- We visit each node once and perform constant-time operations per visit.

**Space Complexity:** `O(n)`  
- Uses additional space to store each node's depth, leading to `O(n)` space complexity.

**Advantage:** This approach avoids recursion depth issues by using a stack, making it suitable for very large or unbalanced trees.

---

### Summary of Approaches:

| Approach                        | Time Complexity | Space Complexity | Description                              |
|---------------------------------|-----------------|------------------|------------------------------------------|
| Recursive DFS                   | `O(n)`          | `O(h)`          | Uses recursion to calculate diameter.    |
| Iterative DFS with Stack        | `O(n)`          | `O(n)`          | Uses stack for depth-first traversal.    |

Both approaches are efficient for calculating the diameter of a binary tree, with the **Iterative DFS** being ideal when avoiding recursion depth limits.