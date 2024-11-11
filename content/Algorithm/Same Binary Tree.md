### Problem
Given the roots of two binary trees `p` and `q`, return `true` if the trees are **equivalent**, otherwise return `false`.

Two binary trees are considered **equivalent** if they share the exact same structure and the nodes have the same values.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/e78fc10c-4692-471f-5261-61e9be4f3a00/public)

```java
Input: p = [1,2,3], q = [1,2,3]

Output: true
```


**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/0b0ee764-c643-46ff-cb3f-86ce8b58ab00/public)

```java
Input: p = [4,7], q = [4,null,7]

Output: false
```


**Example 3:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/4d811f95-0488-490b-1f4f-fc5489df0f00/public)

```java
Input: p = [1,2,3], q = [1,3,2]

Output: false
```


**Constraints:**

- `0 <= The number of nodes in both trees <= 100`.
- `-100 <= Node.val <= 100`

### Solution

**1. Recursive Depth-First Search (DFS) Solution**  
**Explanation:**  
- Two binary trees are considered the same if they have the same structure and all corresponding nodes have the same values.
- Using a recursive DFS approach:
  1. Compare the values of the current nodes in both trees.
  2. Recursively check that both the left subtrees and the right subtrees are also identical.
  3. If both nodes are `null`, they are considered the same at that point. If only one is `null` or their values differ, the trees are not the same.
  
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
     * @param {TreeNode} p
     * @param {TreeNode} q
     * @return {boolean}
     */
    isSameTree(p, q) {
        if (!p && !q) return true; // Both nodes are null, so they are the same
        if (!p || !q || p.val !== q.val) return false; // Either one node is null, or values are different

        return this.isSameTree(p.left, q.left) && this.isSameTree(p.right, q.right);
    }
}
```

**Time Complexity:** `O(n)`  
- We visit each node once, where `n` is the total number of nodes in both trees.

**Space Complexity:** `O(h)`  
- This approach has a space complexity of `O(h)` due to the recursive call stack, where `h` is the height of the tree.

**Drawback:** This solution may lead to stack overflow for very deep trees in languages without tail call optimization.

---

**2. Iterative Depth-First Search with Stack Solution**  
**Explanation:**  
- An iterative approach using a stack can check if two trees are the same:
  1. Initialize a stack with a tuple containing the roots of both trees.
  2. For each node pair in the stack:
     - If both nodes are `null`, continue (they match).
     - If only one is `null` or their values differ, return `false`.
     - Otherwise, push their left and right children as pairs onto the stack.
  3. If we traverse both trees without mismatches, they are identical.

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
     * @param {TreeNode} p
     * @param {TreeNode} q
     * @return {boolean}
     */
    isSameTree(p, q) {
        const stack = [[p, q]];

        while (stack.length > 0) {
            const [node1, node2] = stack.pop();

            if (!node1 && !node2) continue; // Both nodes are null
            if (!node1 || !node2 || node1.val !== node2.val) return false; // Mismatch

            stack.push([node1.left, node2.left]);
            stack.push([node1.right, node2.right]);
        }

        return true;
    }
}
```

**Time Complexity:** `O(n)`  
- Each node is visited once, resulting in a linear time complexity.

**Space Complexity:** `O(n)`  
- The stack could grow to `O(n)` in the case of a completely unbalanced tree.

**Advantage:** This solution avoids recursion depth limitations, making it suitable for trees that may be large or unbalanced.

---

### Summary of Approaches:

| Approach                        | Time Complexity | Space Complexity | Description                                |
|---------------------------------|-----------------|------------------|--------------------------------------------|
| Recursive DFS                   | `O(n)`          | `O(h)`          | Recursively checks if two trees are the same.  |
| Iterative DFS with Stack        | `O(n)`          | `O(n)`          | Uses a stack to check nodes iteratively.       |

The **Iterative DFS** approach is preferred when recursion depth is a concern, as it avoids potential stack overflow, while both methods are efficient and straightforward for determining if two binary trees are the same.