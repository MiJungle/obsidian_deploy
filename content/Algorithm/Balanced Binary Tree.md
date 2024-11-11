### Question
Given a binary tree, return `true` if it is **height-balanced** and `false` otherwise.

A **height-balanced** binary tree is defined as a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/c19c3727-ea28-416c-3873-79ee75f2b400/public)

```java
Input: root = [1,2,3,null,null,4]

Output: true
```


**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/24fcc2da-e012-4f9e-856e-040f200f3c00/public)

```java
Input: root = [1,2,3,null,null,4,null,5]

Output: false
```


**Example 3:**

```java
Input: root = []

Output: true
```


**Constraints:**

- The number of nodes in the tree is in the range `[0, 1000]`.
- `-1000 <= Node.val <= 1000`

### Solution

**1. Recursive Depth-First Search (DFS) Solution**  
**Explanation:**  
- A binary tree is balanced if, for every node, the difference between the heights of the left and right subtrees is no more than 1.
- Using a recursive DFS approach, we can:
  1. Recursively calculate the height of the left and right subtrees for each node.
  2. Check if the difference in height between the two subtrees is greater than 1. If it is, the tree is not balanced.
  3. If any subtree is unbalanced, propagate this result up the recursion chain to indicate that the tree is unbalanced.
- The recursion returns `-1` if the tree is unbalanced at any point, allowing us to detect imbalance early and avoid redundant calculations.

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
     * @return {boolean}
     */
    isBalanced(root) {
        return this.checkHeight(root) !== -1;
    }

    checkHeight(node) {
        if (!node) return 0;

        const leftHeight = this.checkHeight(node.left);
        if (leftHeight === -1) return -1;

        const rightHeight = this.checkHeight(node.right);
        if (rightHeight === -1) return -1;

        if (Math.abs(leftHeight - rightHeight) > 1) return -1;

        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```

**Time Complexity:** `O(n)`  
- We visit each node once to calculate its height and check for balance.

**Space Complexity:** `O(h)`  
- Due to the recursion stack, where `h` is the height of the tree.

**Drawback:** In languages with no tail call optimization, this may cause a stack overflow on very deep trees.

---

**2. Iterative Depth-First Search with Postorder Traversal Solution**  
**Explanation:**  
- We use an iterative approach with a stack and postorder traversal (left-right-root) to calculate the height of subtrees and check for balance in a bottom-up manner.
- For each node:
  1. Check the heights of the left and right subtrees.
  2. If the difference in heights exceeds 1, the tree is unbalanced, so we stop further processing.
- We use a `Map` to store the height of each visited node, allowing us to avoid recomputation.

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
     * @return {boolean}
     */
    isBalanced(root) {
        if (!root) return true;

        const stack = [{ node: root, visited: false }];
        const heightMap = new Map();

        while (stack.length > 0) {
            const { node, visited } = stack.pop();

            if (!node) continue;

            if (visited) {
                const leftHeight = heightMap.get(node.left) || 0;
                const rightHeight = heightMap.get(node.right) || 0;

                if (Math.abs(leftHeight - rightHeight) > 1) return false;

                heightMap.set(node, Math.max(leftHeight, rightHeight) + 1);
            } else {
                stack.push({ node, visited: true });
                if (node.right) stack.push({ node: node.right, visited: false });
                if (node.left) stack.push({ node: node.left, visited: false });
            }
        }

        return true;
    }
}
```

**Time Complexity:** `O(n)`  
- Each node is visited once, and height checks are performed in constant time.

**Space Complexity:** `O(n)`  
- Uses a `Map` to store the height of each node, leading to `O(n)` space complexity.

**Advantage:** This approach avoids recursion stack overflow by using an iterative stack, making it more suitable for large or unbalanced trees.

---

### Summary of Approaches:

| Approach                        | Time Complexity | Space Complexity | Description                              |
|---------------------------------|-----------------|------------------|------------------------------------------|
| Recursive DFS                   | `O(n)`          | `O(h)`          | Uses recursion to check height balance.  |
| Iterative DFS with Stack        | `O(n)`          | `O(n)`          | Uses a stack and `Map` for heights.      |

The **Iterative DFS** is preferred for cases where recursion depth is a concern, as it uses an explicit stack and avoids potential stack overflow. Both methods are efficient for determining if a binary tree is balanced.