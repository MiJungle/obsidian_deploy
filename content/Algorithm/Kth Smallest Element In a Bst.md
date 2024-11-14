### Question
Given the `root` of a binary search tree, and an integer `k`, return the `kth` smallest value (**1-indexed**) in the tree.

A **binary search tree** satisfies the following constraints:

- The left subtree of every node contains only nodes with keys **less than** the node's key.
- The right subtree of every node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees are also binary search trees.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/02eca3db-f72f-4277-7134-faec4f02e500/public)

```java
Input: root = [2,1,3], k = 1

Output: 1
```


**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/dca6c42d-2327-4036-f7f2-3e99d8203100/public)

```java
Input: root = [4,3,5,2,null], k = 4

Output: 5
```


**Constraints:**

- `1 <= k <= The number of nodes in the tree <= 1000`.
- `0 <= Node.val <= 1000`

Here's a solution for the **Kth Smallest Element in a BST** problem on NeetCode in the requested format:

### Solution

**1. In-Order Traversal with Count**  
**Explanation:**  
- Perform an in-order traversal, which visits nodes in ascending order in a BST.
- Use a counter to keep track of the number of nodes visited.
- Stop traversal when the counter reaches `k`, as this will be the `kth` smallest element.
- This approach leverages the BST property, ensuring nodes are visited in sorted order.

**Implementation (JavaScript):**

```javascript
function kthSmallest(root, k) {
    let count = 0;
    let result = null;

    function inOrder(node) {
        if (!node || result !== null) return;

        inOrder(node.left);
        
        count++;
        if (count === k) {
            result = node.val;
            return;
        }

        inOrder(node.right);
    }

    inOrder(root);
    return result;
}
```

**Time Complexity:** `O(k)`  
- The traversal stops once the `kth` element is found, so we only visit up to `k` nodes.

**Space Complexity:** `O(h)`  
- The recursive stack uses space proportional to the tree’s height, `h`.

**Advantage:** Efficient for finding the kth smallest element without traversing the entire tree.

---

**2. Iterative In-Order Traversal Solution**  
**Explanation:**  
- Use an iterative approach with a stack to simulate in-order traversal.
- Initialize a stack and a counter.
- Traverse the left subtree, pushing nodes to the stack until reaching the leftmost node.
- Pop nodes from the stack, incrementing the counter until it matches `k`.
- The `kth` popped node’s value is the `kth` smallest element.

**Implementation (JavaScript):**

```javascript
function kthSmallest(root, k) {
    let stack = [];
    let count = 0;

    while (root || stack.length) {
        while (root) {
            stack.push(root);
            root = root.left;
        }
        
        root = stack.pop();
        count++;
        
        if (count === k) return root.val;
        
        root = root.right;
    }
}
```

**Time Complexity:** `O(k)`  
- We only pop `k` nodes, so traversal stops early.

**Space Complexity:** `O(h)`  
- Stack space usage is proportional to the height of the tree, `h`.

**Advantage:** This iterative solution avoids potential issues with deep recursion, making it suitable for larger trees.

---

### Summary of Approaches

| Approach               | Time Complexity | Space Complexity | Description                          |
|------------------------|-----------------|------------------|--------------------------------------|
| Recursive In-Order     | `O(k)`         | `O(h)`          | Recursive in-order with early exit   |
| Iterative In-Order     | `O(k)`         | `O(h)`          | Iterative traversal with a stack     |

The **iterative in-order solution** is efficient for large trees, while the **recursive solution** is concise and easy to understand. Both approaches are optimal for finding the kth smallest element in a BST.