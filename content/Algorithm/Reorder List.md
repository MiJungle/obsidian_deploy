### Question
You are given the head of a singly linked-list.

The positions of a linked list of `length = 7` for example, can intially be represented as:

`[0, 1, 2, 3, 4, 5, 6]`

Reorder the nodes of the linked list to be in the following order:

`[0, 6, 1, 5, 2, 4, 3]`

Notice that in the general case for a list of `length = n` the nodes are reordered to be in the following order:

`[0, n-1, 1, n-2, 2, n-3, ...]`

You may not modify the values in the list's nodes, but instead you must reorder the nodes themselves.

**Example 1:**

```java
Input: head = [2,4,6,8]

Output: [2,8,4,6]
```

**Example 2:**

```java
Input: head = [2,4,6,8,10]

Output: [2,10,4,8,6]
```

**Constraints:**

- `1 <= Length of the list <= 1000`.
- `1 <= Node.val <= 1000`


### Solution

**1. Optimal Solution with Three Steps (Find Middle, Reverse Second Half, Merge)**  
**Explanation:**  
- **Step 1**: Use the slow and fast pointer approach to find the middle of the list.
- **Step 2**: Reverse the second half of the list.
- **Step 3**: Merge the two halves by alternating nodes from each half.

**Implementation (JavaScript):**

```javascript
function reorderList(head) {
    if (!head || !head.next) return;

    // Step 1: Find the middle of the list
    let slow = head, fast = head;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }

    // Step 2: Reverse the second half
    let prev = null, current = slow.next;
    slow.next = null; // Split the list into two halves
    while (current) {
        let nextNode = current.next;
        current.next = prev;
        prev = current;
        current = nextNode;
    }

    // Step 3: Merge the two halves
    let first = head, second = prev;
    while (second) {
        let temp1 = first.next;
        let temp2 = second.next;
        first.next = second;
        second.next = temp1;
        first = temp1;
        second = temp2;
    }
}
```

**Time Complexity:** `O(n)`  
- `n`: Number of nodes in the linked list.

**Space Complexity:** `O(1)`  
- In-place, using only a few pointers.

**Advantage:** Efficient in both time and space complexity due to constant space usage.

---

**2. Using an Array to Store Nodes**  
**Explanation:**  
- Store all nodes in an array.
- Use two pointers (one starting at the beginning and one at the end) to rearrange nodes alternately.
- Reconnect nodes as per the reordered sequence.

**Implementation (JavaScript):**

```javascript
function reorderList(head) {
    if (!head || !head.next) return;

    // Step 1: Store nodes in an array
    const nodes = [];
    let current = head;
    while (current) {
        nodes.push(current);
        current = current.next;
    }

    // Step 2: Reorder list using two pointers
    let left = 0, right = nodes.length - 1;
    while (left < right) {
        nodes[left].next = nodes[right];
        left++;
        if (left === right) break;
        nodes[right].next = nodes[left];
        right--;
    }
    nodes[left].next = null; // Set the last node's next to null
}
```

**Time Complexity:** `O(n)`  
**Space Complexity:** `O(n)`  
- Uses extra space to store nodes in an array.

**Advantage:** Conceptually simpler and easier to understand than the pointer-based approach.

---

**3. Recursive Solution**  
**Explanation:**  
- Use recursion to traverse to the end of the list, keeping track of the nodes from the beginning.
- For each recursive call, reorder nodes until the middle of the list is reached.

**Implementation (JavaScript):**

```javascript
function reorderList(head) {
    let left = head;

    function reorder(right) {
        if (!right) return;
        reorder(right.next);

        if (left.next && left !== right && left.next !== right) {
            const nextLeft = left.next;
            left.next = right;
            right.next = nextLeft;
            left = nextLeft;
        } else {
            right.next = null;
        }
    }

    reorder(head);
}
```

**Time Complexity:** `O(n)`  
**Space Complexity:** `O(n)`  
- Recursive calls add space complexity due to the call stack.

**Advantage:** Elegant solution using recursion, though it may be less space-efficient for large lists.

---

### Summary of Approaches

| Approach                 | Time Complexity | Space Complexity | Description                                 |
|--------------------------|-----------------|------------------|---------------------------------------------|
| Three-Step Optimal       | `O(n)`         | `O(1)`           | Uses slow/fast pointers, reverses second half, and merges |
| Array-Based              | `O(n)`         | `O(n)`           | Uses an array to simplify reordering        |
| Recursive                | `O(n)`         | `O(n)`           | Recursive solution that traverses end-to-start |

The **Three-Step Optimal solution** is generally preferred due to its time and space efficiency, especially for large inputs. The **array-based** method is also straightforward and intuitive if space complexity is not an issue.