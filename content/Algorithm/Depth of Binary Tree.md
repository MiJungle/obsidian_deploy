### Question
Given the `root` of a binary tree, return its **depth**.

The **depth** of a binary tree is defined as the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/5ea6da77-7e43-43e0-dd9d-e879ca0b1600/public)

```java
Input: root = [1,2,3,null,null,4]

Output: 3
```

**Example 2:**

```java
Input: root = []

Output: 0
```

**Constraints:**

- `0 <= The number of nodes in the tree <= 100`.
- `-100 <= Node.val <= 100`



### Solution

**1. Recursive Depth-First Search (DFS) Solution**  
**Explanation:**  
- The maximum depth of a binary tree is the longest path from the root node to any leaf node.
- Using a recursive approach, we can determine the maximum depth by:
  1. Calculating the depth of the left subtree and right subtree of each node.
  2. Returning the greater depth between the two, adding 1 for the current node.

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
    maxDepth(root) {
        if (!root) return 0; // Base case: empty node has depth 0

        const leftDepth = this.maxDepth(root.left);
        const rightDepth = this.maxDepth(root.right);

        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```

**Time Complexity:** `O(n)`  
- We visit each node once to calculate its depth.

**Space Complexity:** `O(h)`  
- This approach has a space complexity of `O(h)` due to the recursive stack, where `h` is the height of the tree. In the worst case (an unbalanced tree), `h` could be `n`.

**Drawback:** This solution may cause a stack overflow in languages without tail call optimization on very deep trees due to recursion depth limits.

---

**2. Iterative Breadth-First Search (BFS) Solution**  
**Explanation:**  
- Using a queue, we can traverse the tree level by level:
  1. Each time we process a level completely, we increment the depth count.
  2. By the end, the depth count will equal the maximum depth of the tree.

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
    maxDepth(root) {
        if (!root) return 0;

        let depth = 0;
        const queue = [root];

        while (queue.length > 0) {
            depth++;
            let levelSize = queue.length;
            
            for (let i = 0; i < levelSize; i++) {
                const node = queue.shift();
                if (node.left) queue.push(node.left);
                if (node.right) queue.push(node.right);
            }
        }

        return depth;
    }
}
```

**Time Complexity:** `O(n)`  
- Each node is visited once as we traverse level by level.

**Space Complexity:** `O(n)`  
- The maximum space is proportional to the number of nodes at the widest part of the tree, which is `O(n)` in the worst case.

**Advantage:** This approach avoids recursion stack overflow issues, making it suitable for trees with a large height.

---

### Summary of Approaches:

| Approach                     | Time Complexity | Space Complexity | Description                            |
|------------------------------|-----------------|------------------|----------------------------------------|
| Recursive DFS                | `O(n)`          | `O(h)`          | Uses recursion to calculate max depth. |
| Iterative BFS                | `O(n)`          | `O(n)`          | Uses a queue to count levels iteratively. |

**Iterative BFS** is often preferred in cases where recursion depth is a concern, as it avoids potential stack overflow and has a manageable memory footprint. Both methods, however, are efficient for calculating the maximum depth of a binary tree.