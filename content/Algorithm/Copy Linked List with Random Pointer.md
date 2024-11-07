### Question
You are given the head of a linked list of length `n`. Unlike a singly linked list, each node contains an additional pointer `random`, which may point to any node in the list, or `null`.

Create a **deep copy** of the list.

The deep copy should consist of exactly `n` **new** nodes, each including:

- The original value `val` of the copied node
- A `next` pointer to the new node corresponding to the `next` pointer of the original node
- A `random` pointer to the new node corresponding to the `random` pointer of the original node

Note: None of the pointers in the new list should point to nodes in the original list.

_Return the head of the copied linked list._

In the examples, the linked list is represented as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where `random_index` is the index of the node (0-indexed) that the `random` pointer points to, or `null` if it does not point to any node.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/5a5c2bdd-51e2-4795-4544-096af4b6cc00/public)

```java
Input: head = [[3,null],[7,3],[4,0],[5,1]]

Output: [[3,null],[7,3],[4,0],[5,1]]
```

Copy

**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/6e56fa98-cf1e-4ca6-18d4-716dac4ba900/public)

```java
Input: head = [[1,null],[2,2],[3,2]]

Output: [[1,null],[2,2],[3,2]]
```

Copy

**Constraints:**

- `0 <= n <= 100`
- `-100 <= Node.val <= 100`
- `random` is `null` or is pointing to some node in the linked list.


### Solution
### Solution: Copy Linked List with Random Pointer

**1. Brute Force Solution: Using a Hash Map**  
**Explanation:**  
- Traverse the original list and create a copy of each node in a new list.
- Use a hash map to map each node from the original list to its copy.
- After creating the new list, traverse the original list again to set the `random` pointers in the new list using the hash map.

**Implementation (JavaScript):**

```javascript
class Node {
    constructor(val = 0, next = null, random = null) {
        this.val = val;
        this.next = next;
        this.random = random;
    }
}

class Solution {
    /**
     * @param {Node} head
     * @return {Node}
     */
    copyRandomList(head) {
        if (!head) return null;

        // Step 1: Create a map to store original nodes and their copies
        const map = new Map();
        let node = head;

        // Step 2: Create a copy of each node and store in the map
        while (node) {
            map.set(node, new Node(node.val));
            node = node.next;
        }

        // Step 3: Set next and random pointers for the copied nodes
        node = head;
        while (node) {
            if (node.next) {
                map.get(node).next = map.get(node.next);
            }
            if (node.random) {
                map.get(node).random = map.get(node.random);
            }
            node = node.next;
        }

        // Return the copied head node
        return map.get(head);
    }
}
```

**Time Complexity:** `O(n)`  
- We traverse the list twice: once to create the nodes and once to assign the `next` and `random` pointers.

**Space Complexity:** `O(n)`  
- We use a hash map to store each node and its copy.

**Drawback:** This approach requires extra space for the hash map, which can be a concern if memory usage is limited.

---

**2. Optimized Solution: In-Place Solution with Constant Space**  
**Explanation:**  
- We copy each node and insert it right after the original node.
- After inserting the copy nodes, we can set the `random` pointers using the newly inserted nodes.
- Finally, we separate the copied list from the original list and return the head of the copied list.

**Implementation (JavaScript):**

```javascript
class Node {
    constructor(val = 0, next = null, random = null) {
        this.val = val;
        this.next = next;
        this.random = random;
    }
}

class Solution {
    /**
     * @param {Node} head
     * @return {Node}
     */
    copyRandomList(head) {
        if (!head) return null;

        let node = head;

        // Step 1: Create copy nodes and insert them right after the original nodes
        while (node) {
            const copy = new Node(node.val);
            copy.next = node.next;
            node.next = copy;
            node = copy.next;
        }

        // Step 2: Set the random pointers of the copied nodes
        node = head;
        while (node) {
            if (node.random) {
                node.next.random = node.random.next;
            }
            node = node.next.next;
        }

        // Step 3: Separate the original list from the copied list
        node = head;
        const newHead = head.next;
        while (node) {
            const copy = node.next;
            node.next = copy.next;
            if (copy.next) copy.next = copy.next.next;
            node = node.next;
        }

        // Return the copied list's head
        return newHead;
    }
}
```

**Time Complexity:** `O(n)`  
- We traverse the list three times: once to copy the nodes, once to set the `random` pointers, and once to separate the two lists.

**Space Complexity:** `O(1)`  
- This approach uses constant space since we only modify the original list in place without needing extra data structures.

**Advantage:** This solution is optimal in terms of space complexity, as it doesn't require additional memory like the hash map solution.

---

### Summary of Approaches:

| Approach                    | Time Complexity       | Space Complexity      | Description                        |
|-----------------------------|-----------------------|-----------------------|------------------------------------|
| Brute Force (Hash Map)      | `O(n)`                | `O(n)`                | Use a hash map to store node copies and their random pointers. |
| In-Place (Constant Space)   | `O(n)`                | `O(1)`                | Copy nodes in place and set random pointers, then separate lists. |

**In-Place Solution** is the most efficient in terms of space, as it does not require additional memory structures like the hash map, making it the preferred approach for copying a linked list with random pointers.