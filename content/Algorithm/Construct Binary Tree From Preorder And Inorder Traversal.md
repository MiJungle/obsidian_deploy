You are given two integer arrays `preorder` and `inorder`.

- `preorder` is the preorder traversal of a binary tree
- `inorder` is the inorder traversal of the same tree
- Both arrays are of the same size and consist of unique values.

Rebuild the binary tree from the preorder and inorder traversals and return its root.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/938c14d3-6669-47ab-924b-a1a08640f200/public)

```java
Input: preorder = [1,2,3,4], inorder = [2,1,3,4]

Output: [1,2,3,null,null,null,4]
```


**Example 2:**

```java
Input: preorder = [1], inorder = [1]

Output: [1]
```


**Constraints:**

- `1 <= inorder.length <= 1000`.
- `inorder.length == preorder.length`
- `-1000 <= preorder[i], inorder[i] <= 1000`

Here’s a solution for the **Construct Binary Tree from Preorder and Inorder Traversal** problem on NeetCode in the requested format:

### Solution

**1. Recursive Divide-and-Conquer Solution**  
**Explanation:**  
- In the preorder traversal, the first element is always the root of the tree.
- In the inorder traversal, elements to the left of the root are in the left subtree, and elements to the right are in the right subtree.
- Use this information to recursively construct the tree:
  - Find the root from the start of the preorder array.
  - Locate this root in the inorder array to split it into left and right subtrees.
  - Recur on each subtree using the respective sections of preorder and inorder arrays.
  
**Implementation (JavaScript):**

```javascript
function buildTree(preorder, inorder) {
    const inorderMap = new Map();
    inorder.forEach((val, index) => inorderMap.set(val, index));

    let preIndex = 0;

    function arrayToTree(left, right) {
        if (left > right) return null;

        const rootVal = preorder[preIndex++];
        const root = new TreeNode(rootVal);

        root.left = arrayToTree(left, inorderMap.get(rootVal) - 1);
        root.right = arrayToTree(inorderMap.get(rootVal) + 1, right);

        return root;
    }

    return arrayToTree(0, inorder.length - 1);
}
```

**Time Complexity:** `O(n)`  
- Each node is visited once, where `n` is the number of nodes.

**Space Complexity:** `O(n)`  
- The recursion stack could go up to `O(n)` in the worst case for a skewed tree, plus the map storage.

**Advantage:** Efficiently constructs the tree using preorder and inorder indices, avoiding unnecessary slicing of arrays.

---

**2. Iterative Solution Using a Stack**  
**Explanation:**  
- This approach simulates the recursive process iteratively with a stack.
- Keep track of the `preorder` index to identify the next root.
- Use a stack to help in constructing the tree as follows:
  - Push nodes onto the stack as long as they are on the left.
  - When a node can no longer be added to the left, pop nodes to find the correct right child position.
- This approach is trickier but can be efficient for large trees by avoiding recursion.

**Implementation (JavaScript):**

```javascript
function buildTree(preorder, inorder) {
    if (!preorder.length || !inorder.length) return null;

    const root = new TreeNode(preorder[0]);
    const stack = [root];
    let inIndex = 0;

    for (let i = 1; i < preorder.length; i++) {
        let node = stack[stack.length - 1];
        const newNode = new TreeNode(preorder[i]);

        if (node.val !== inorder[inIndex]) {
            node.left = newNode;
        } else {
            while (stack.length && stack[stack.length - 1].val === inorder[inIndex]) {
                node = stack.pop();
                inIndex++;
            }
            node.right = newNode;
        }
        stack.push(newNode);
    }

    return root;
}
```

**Time Complexity:** `O(n)`  
- Each node is processed once.

**Space Complexity:** `O(n)`  
- Stack space for `n` nodes, as well as for creating nodes.

**Advantage:** Avoids recursion depth issues and can be more intuitive in certain use cases.

---

### Summary of Approaches

| Approach                    | Time Complexity | Space Complexity | Description                          |
|-----------------------------|-----------------|------------------|--------------------------------------|
| Recursive Divide-and-Conquer | `O(n)`         | `O(n)`          | Builds tree using preorder and inorder indices |
| Iterative Stack Solution     | `O(n)`         | `O(n)`          | Uses a stack to simulate recursion   |

The **recursive divide-and-conquer solution** is usually simpler and more intuitive, while the **iterative stack solution** is useful for handling larger trees where recursion depth could be a limitation.