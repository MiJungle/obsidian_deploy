### Question
Given a binary tree `root`, return the level order traversal of it as a nested list, where each sublist contains the values of nodes at a particular level in the tree, from left to right.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/a4639809-0754-4eda-221f-a4cd58bd9c00/public)

```java
Input: root = [1,2,3,4,5,6,7]

Output: [[1],[2,3],[4,5,6,7]]
```


**Example 2:**

```java
Input: root = [1]

Output: [[1]]
```


**Example 3:**

```java
Input: root = []

Output: []
```


**Constraints:**

- `0 <= The number of nodes in both trees <= 1000`.
- `-1000 <= Node.val <= 1000`

Here's a structured solution for the **Level Order Traversal of a Binary Tree** problem:

---

### Solution

**1. Iterative Solution Using a Queue**  
**Explanation:**  
- This approach uses a queue to traverse the tree level by level.
- Start by enqueuing the root node.
- For each level:
  - Track the number of nodes at the current level.
  - Dequeue each node, add its value to a temporary list, and enqueue its children.
  - After processing all nodes at the current level, add the temporary list to the result list.
- Repeat until all levels are processed.

**Implementation (JavaScript):**

```javascript
function levelOrder(root) {
    if (!root) return [];

    const result = [];
    const queue = [root];

    while (queue.length > 0) {
        const levelSize = queue.length;
        const currentLevel = [];

        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift();
            currentLevel.push(node.val);

            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }

        result.push(currentLevel);
    }

    return result;
}
```

**Time Complexity:** `O(n)`  
- `n`: Number of nodes in the tree.
- Each node is visited once, so it’s linear in the number of nodes.

**Space Complexity:** `O(n)`  
- The maximum space usage is proportional to the number of nodes at the widest level (in the queue).

**Advantage:** Efficiently processes each level using a queue, suitable for wide or large trees.

---

**2. Recursive Solution Using Depth (DFS)**  
**Explanation:**  
- Use depth-first search (DFS) to reach each level, while maintaining a `result` list where each index corresponds to a level.
- For each node, pass its depth as an argument:
  - If the `result` list doesn’t have a sublist for the current depth, add a new sublist.
  - Append the node's value to the appropriate depth's sublist.
- This method builds the level order traversal by recursive depth-first traversal.

**Implementation (JavaScript):**

```javascript
function levelOrder(root) {
    const result = [];
    dfs(root, 0, result);
    return result;
}

function dfs(node, level, result) {
    if (!node) return;

    if (result.length === level) {
        result.push([]);
    }

    result[level].push(node.val);
    dfs(node.left, level + 1, result);
    dfs(node.right, level + 1, result);
}
```

**Time Complexity:** `O(n)`  
- Each node is visited once.

**Space Complexity:** `O(h)`  
- `h`: Height of the tree (recursive call stack).

**Advantage:** Elegant and concise, leveraging recursion to manage levels naturally.

---

**3. Zigzag Level Order Traversal (for Variation)**  
**Explanation:**  
- This variation performs a level order traversal but alternates the order of each level.
- Using a queue, keep track of the current level’s order:
  - If it's a left-to-right level, add nodes as usual.
  - If it's a right-to-left level, add nodes in reverse order.
- Alternate the order after each level to achieve a zigzag pattern.

**Implementation (JavaScript):**

```javascript
function zigzagLevelOrder(root) {
    if (!root) return [];

    const result = [];
    const queue = [root];
    let leftToRight = true;

    while (queue.length > 0) {
        const levelSize = queue.length;
        const currentLevel = [];

        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift();
            if (leftToRight) {
                currentLevel.push(node.val);
            } else {
                currentLevel.unshift(node.val);
            }

            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }

        result.push(currentLevel);
        leftToRight = !leftToRight;
    }

    return result;
}
```

**Time Complexity:** `O(n)`  
**Space Complexity:** `O(n)`  

**Advantage:** Useful for applications needing alternating level orders, adding flexibility to level-order traversal.

---

### Summary of Approaches

| Approach                 | Time Complexity | Space Complexity | Description                                      |
|--------------------------|-----------------|------------------|--------------------------------------------------|
| Iterative Solution (Queue)| `O(n)`         | `O(n)`          | Uses a queue to process levels one by one        |
| Recursive Solution (DFS)  | `O(n)`         | `O(h)`          | Uses DFS with depth tracking for level structure |
| Zigzag Level Order        | `O(n)`         | `O(n)`          | Alternates level orders for a zigzag pattern     |

The **iterative solution** is often the most efficient for standard level order traversal, while the **recursive solution** provides a cleaner, more concise approach. The **zigzag solution** adds versatility for specific traversal patterns.
