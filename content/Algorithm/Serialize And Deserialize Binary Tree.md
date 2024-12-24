### Question
Implement an algorithm to serialize and deserialize a binary tree.

Serialization is the process of converting an in-memory structure into a sequence of bits so that it can be stored or sent across a network to be reconstructed later in another computer environment.

You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure. There is no additional restriction on how your serialization/deserialization algorithm should work.

**Note:** The input/output format in the examples is the same as how NeetCode serializes a binary tree. You do not necessarily need to follow this format.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/a9dfb17f-70e9-42a3-ba97-33cfd82f6100/public)

```java
Input: root = [1,2,3,null,null,4,5]

Output: [1,2,3,null,null,4,5]
```


**Example 2:**

```java
Input: root = []

Output: []
```

**Constraints:**

- `0 <= The number of nodes in the tree <= 1000`.
- `-1000 <= Node.val <= 1000`




### Step by Step

Let's break down the process step by step to understand how the `serialize` and `deserialize` functions work and determine left and right children during deserialization.

---

### 1. Serialization (`serialize`)

The goal is to convert the binary tree into a string that represents its structure and values using **preorder traversal** (root, left, right). The traversal captures the order of nodes and uses a placeholder (`'N'`) for `null` nodes.

#### Key Steps:

1. **Start at the root**:
    
    - Add the root's value to the result array.
    - Recursively process the left and right subtrees.
2. **Handle `null` nodes**:
    
    - When encountering a `null` child, add `'N'` to the result array to represent that no node exists in that position.
3. **Output**:
    
    - Return a single string representing the tree, created by joining the result array with commas.

#### Example:

Tree:

```
       1
      / \
     2   3
        / \
       4   5
```

Step-by-step `serialize`:

1. Start at `1`: Add `1` → Result: `['1']`
2. Go left to `2`: Add `2` → Result: `['1', '2']`
3. `2` has no children: Add `N, N` → Result: `['1', '2', 'N', 'N']`
4. Go back to `1`, then right to `3`: Add `3` → Result: `['1', '2', 'N', 'N', '3']`
5. Go left to `4`: Add `4` → Result: `['1', '2', 'N', 'N', '3', '4']`
6. `4` has no children: Add `N, N` → Result: `['1', '2', 'N', 'N', '3', '4', 'N', 'N']`
7. Go back to `3`, then right to `5`: Add `5` → Result: `['1', '2', 'N', 'N', '3', '4', 'N', 'N', '5']`
8. `5` has no children: Add `N, N` → Result: `['1', '2', 'N', 'N', '3', '4', 'N', 'N', '5', 'N', 'N']`

Final serialized string: `"1,2,N,N,3,4,N,N,5,N,N"`

---

### 2. Deserialization (`deserialize`)

The goal is to reconstruct the binary tree from the serialized string. The key is to process the string in the **same order** it was serialized (preorder traversal).

#### Key Steps:

1. **Split the string into an array**:
    
    - Each element represents a node value or a `null` placeholder (`'N'`).
2. **Track the current position in the array**:
    
    - Use an index pointer (`i.val`) to keep track of which value is being processed.
3. **Rebuild the tree recursively**:
    
    - If the current value is `'N'`, return `null` (no node exists).
    - Otherwise:
        - Create a `TreeNode` with the current value.
        - Recursively call the function for the left child.
        - Recursively call the function for the right child.
4. **Determining left and right**:
    
    - The recursive function processes the values **in the order they appear**.
    - Since the string was built using preorder traversal, the left child always comes before the right child in the serialized data.

---

#### Example:

Serialized string: `"1,2,N,N,3,4,N,N,5,N,N"`

Step-by-step `deserialize`:

1. Start with the first value (`1`): Create a root node `1` → Move to the next value.
2. Process the left child (`2`): Create a node `2` → Move to the next value.
3. Next value is `'N'`: `2` has no left child → Move to the next value.
4. Next value is `'N'`: `2` has no right child → Move to the next value.
5. Go back to `1`, process the right child (`3`): Create a node `3` → Move to the next value.
6. Process the left child of `3` (`4`): Create a node `4` → Move to the next value.
7. Next value is `'N'`: `4` has no left child → Move to the next value.
8. Next value is `'N'`: `4` has no right child → Move to the next value.
9. Go back to `3`, process the right child (`5`): Create a node `5` → Move to the next value.
10. Next value is `'N'`: `5` has no left child → Move to the next value.
11. Next value is `'N'`: `5` has no right child → Done.

Reconstructed tree:

```
       1
      / \
     2   3
        / \
       4   5
```

---

### How Left and Right Are Determined

- The serialized string uses **preorder traversal** (root, left, right).
- During deserialization:
    1. Process the root node first.
    2. The **next value in the array** represents the left child.
    3. After finishing the left subtree, the next value represents the right child.

The preorder order ensures the left and right children are processed in the correct sequence.