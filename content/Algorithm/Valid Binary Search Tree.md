### Question
Given the `root` of a binary tree, return `true` if it is a **valid binary search tree**, otherwise return `false`.

A **valid binary search tree** satisfies the following constraints:

- The left subtree of every node contains only nodes with keys **less than** the node's key.
- The right subtree of every node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees are also binary search trees.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/18f9a316-8dc2-4e11-d304-51204454ac00/public)

```java
Input: root = [2,1,3]

Output: true
```


**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/6f14cb8d-efad-4221-2beb-fba2b19c8a00/public)

```java
Input: root = [1,2,3]

Output: false
```


Explanation: The root node's value is 1 but its left child's value is 2 which is greater than 1.

**Constraints:**

- `1 <= The number of nodes in the tree <= 1000`.
- `-1000 <= Node.val <= 1000`

Here’s a solution to the **Valid Binary Search Tree** problem on NeetCode in a similar format:

### Solution

**1. Recursive Depth-First Search (DFS) Solution**  
**Explanation:**  
- Use a recursive DFS approach to verify that each node satisfies the BST property.
- Pass down `min` and `max` constraints for each node:
  - The `left` child must be less than the current node’s value, and the `right` child must be greater.
- Recursively validate each subtree by updating these constraints:
  - For the left subtree, set `max` as the current node’s value.
  - For the right subtree, set `min` as the current node’s value.
- If a node does not satisfy its constraints, return `false`.

**Implementation (JavaScript):**

```javascript
function isValidBST(root, min = -Infinity, max = Infinity) {
    if (root === null) return true;

    if (root.val <= min || root.val >= max) return false;

    return isValidBST(root.left, min, root.val) && 
           isValidBST(root.right, root.val, max);
}
```

**Time Complexity:** `O(n)`  
- Each node is visited once.

**Space Complexity:** `O(h)`  
- Where `h` is the height of the tree, due to recursive stack usage.

**Advantage:** Simple to implement and efficient for most tree structures.

---

**2. Iterative In-Order Traversal Solution**  
**Explanation:**  
- Use in-order traversal, which should yield values in ascending order for a valid BST.
- Use a stack to perform an iterative in-order traversal, tracking the previous node's value.
- During traversal:
  - Compare the current node’s value with the previous node’s value.
  - If the current node is not greater, return `false`.
- If all nodes are in ascending order, the tree is a valid BST.

**Implementation (JavaScript):**

```javascript
function isValidBST(root) {
    let stack = [];
    let prev = -Infinity;

    while (stack.length || root) {
        while (root) {
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();

        if (root.val <= prev) return false;
        prev = root.val;

        root = root.right;
    }
    return true;
}
```

**Time Complexity:** `O(n)`  
- Each node is visited once.

**Space Complexity:** `O(h)`  
- The stack may store up to `h` nodes, where `h` is the height of the tree.

**Advantage:** Avoids recursion depth limits, making it suitable for deep trees.

---

### Summary of Approaches

| Approach              | Time Complexity | Space Complexity | Description                          |
|-----------------------|-----------------|------------------|--------------------------------------|
| Recursive DFS         | `O(n)`         | `O(h)`          | Checks constraints recursively       |
| Iterative In-Order    | `O(n)`         | `O(h)`          | In-order traversal with stack        |

The **recursive DFS** approach is typically simpler and effective for smaller trees, while the **iterative in-order solution** avoids potential recursion depth issues in very deep trees.
