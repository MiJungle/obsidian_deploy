Given a binary search tree (BST) where all node values are _unique_, and two nodes from the tree `p` and `q`, return the lowest common ancestor (LCA) of the two nodes.

The lowest common ancestor between two nodes `p` and `q` is the lowest node in a tree `T` such that both `p` and `q` as descendants. The ancestor is allowed to be a descendant of itself.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/2080ee6a-3d27-4cd5-0db2-07672ead8200/public)

```java
Input: root = [5,3,8,1,4,7,9,null,2], p = 3, q = 8

Output: 5
```


**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/2080ee6a-3d27-4cd5-0db2-07672ead8200/public)

```java
Input: root = [5,3,8,1,4,7,9,null,2], p = 3, q = 4

Output: 3
```


Explanation: The LCA of nodes 3 and 4 is 3, since a node can be a descendant of itself.

**Constraints:**

- `2 <= The number of nodes in the tree <= 100`.
- `-100 <= Node.val <= 100`
- `p != q`
- `p` and `q` will both exist in the BST.


### Solution
Here’s a structured solution for the **Lowest Common Ancestor in a Binary Search Tree (BST)** problem:

---

### Solution

**1. Iterative Solution Using BST Properties**  
**Explanation:**  
- In a Binary Search Tree, for any two nodes `p` and `q`, their lowest common ancestor (`LCA`) will be the first node where `p` and `q` fall on different sides (one in the left subtree and the other in the right) or the node itself if it is equal to either `p` or `q`.
- Start at the root and iterate down the tree:
  - If both `p` and `q` values are less than the current node’s value, move to the left subtree.
  - If both are greater, move to the right subtree.
  - If they’re on different sides, the current node is the `LCA`.

**Implementation (JavaScript):**

```javascript
function lowestCommonAncestor(root, p, q) {
    while (root) {
        if (p.val < root.val && q.val < root.val) {
            root = root.left;
        } else if (p.val > root.val && q.val > root.val) {
            root = root.right;
        } else {
            return root;  // This is the LCA node
        }
    }
    return null;
}
```

**Time Complexity:** `O(h)`  
- `h`: Height of the tree.
- Only one path from the root to the LCA node is traversed, making it efficient.

**Space Complexity:** `O(1)`  
- No additional space is used beyond a few pointers.

**Advantage:** Efficient and avoids recursion, making it suitable for deep trees.

---

**2. Recursive Solution Using BST Properties**  
**Explanation:**  
- Use the BST property recursively to identify the split point where `p` and `q` fall on opposite sides.
- If both `p` and `q` are smaller than the current node, recurse on the left subtree.
- If both are larger, recurse on the right subtree.
- When they’re on opposite sides, or one is the current node itself, it’s the `LCA`.

**Implementation (JavaScript):**

```javascript
function lowestCommonAncestor(root, p, q) {
    if (p.val < root.val && q.val < root.val) {
        return lowestCommonAncestor(root.left, p, q);
    } else if (p.val > root.val && q.val > root.val) {
        return lowestCommonAncestor(root.right, p, q);
    } else {
        return root;  // This is the LCA node
    }
}
```

**Time Complexity:** `O(h)`  
**Space Complexity:** `O(h)`  
- The recursive call stack may go up to the height of the tree.

**Advantage:** Simple and elegant, with efficient recursive traversal based on BST properties.

---

**3. Generalized Solution for Binary Trees (Without BST Properties)**  
**Explanation:**  
- This solution works for a general binary tree where we don’t have BST properties to rely on.
- Traverse the tree recursively:
  - If the current node matches `p` or `q`, return the current node.
  - Recursively search for `p` and `q` in both left and right subtrees.
  - If `p` and `q` are found in different subtrees, the current node is the `LCA`.
  - If both nodes are in one subtree, return the found node.

**Implementation (JavaScript):**

```javascript
function lowestCommonAncestor(root, p, q) {
    if (!root || root === p || root === q) return root;

    const left = lowestCommonAncestor(root.left, p, q);
    const right = lowestCommonAncestor(root.right, p, q);

    if (left && right) return root;  // Both nodes found in different subtrees
    return left ? left : right;  // Return the non-null node
}
```

**Time Complexity:** `O(n)`  
- `n`: Number of nodes in the tree.
- This solution needs to potentially check each node once.

**Space Complexity:** `O(h)`  
- The recursive stack may go up to the height of the tree.

**Advantage:** Works for both BSTs and regular binary trees, making it versatile.

---

### Summary of Approaches

| Approach                         | Time Complexity | Space Complexity | Description                                         |
|----------------------------------|-----------------|------------------|-----------------------------------------------------|
| Iterative Solution Using BST     | `O(h)`         | `O(1)`          | Uses a loop based on BST properties for efficiency  |
| Recursive Solution Using BST     | `O(h)`         | `O(h)`          | Recursively finds LCA based on BST properties       |
| General Solution for Binary Tree | `O(n)`         | `O(h)`          | Works for any binary tree without relying on BST properties |

For a **Binary Search Tree**, the **iterative solution** is often the most efficient, avoiding recursion while leveraging BST properties. For a **general binary tree**, the **generalized recursive solution** should be used.