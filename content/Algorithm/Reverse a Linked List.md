### Question
Given the beginning of a singly linked list `head`, reverse the list, and return the new beginning of the list.

**Example 1:**

```java
Input: head = [0,1,2,3]

Output: [3,2,1,0]
```

**Example 2:**

```java
Input: head = []

Output: []
```

**Constraints:**

- `0 <= The length of the list <= 1000`.
- `-1000 <= Node.val <= 1000`

To reverse a linked list, here are three JavaScript solutions, ranging from iterative to recursive approaches:


### Solution

**1. Iterative Solution**  
**Explanation:**  
- Start with three pointers: `prev` (initially null), `current` (the head of the list), and `next`.
- Traverse the list while reversing the `next` pointer of each node to point to `prev`.
- Move all three pointers forward until you reach the end of the list.

**Implementation (JavaScript):**

```javascript
function reverseList(head) {
    let prev = null;
    let current = head;
    while (current !== null) {
        let next = current.next; // Save the next node
        current.next = prev;     // Reverse the link
        prev = current;          // Move prev to current
        current = next;          // Move to the next node
    }
    return prev; // New head of the reversed list
}
```

**Time Complexity:** `O(n)`  
- `n`: Number of nodes in the linked list. Each node is visited once.

**Space Complexity:** `O(1)`  
- Only a few pointers are used, making it a constant space solution.

**Advantage:** Efficient and simple, often the preferred method for reversing a linked list.

---

**2. Recursive Solution**  
**Explanation:**  
- Recursively traverse to the end of the list.
- As each recursive call returns, reverse the link between the nodes, setting `next` of the current node to point to the previous one.
- Base case is when we reach the last node, which becomes the new head.

**Implementation (JavaScript):**

```javascript
function reverseList(head) {
    // Base case: if head is null or only one node, return it
    if (head === null || head.next === null) return head;

    // Recurse to the end of the list
    const newHead = reverseList(head.next);
    
    // Reverse the link of the current node
    head.next.next = head;
    head.next = null; // Set the current node's next to null

    return newHead; // Return the new head from the end of the list
}
```

**Time Complexity:** `O(n)`  
- `n`: Number of nodes, as each node is visited once in the recursive calls.

**Space Complexity:** `O(n)`  
- Recursion stack takes up space for each node.

**Advantage:** This method is elegant but may not be ideal for very large lists due to recursion depth limits.

---

**3. Tail Recursive Solution**  
**Explanation:**  
- This is a variation of the recursive approach that avoids extra space usage by passing `prev` as a parameter.
- The function passes `prev` and `current.next` in each recursive call, effectively reversing links while unwinding the stack.

**Implementation (JavaScript):**

```javascript
function reverseList(head, prev = null) {
    // Base case: if head is null, return prev as the new head
    if (head === null) return prev;

    let next = head.next;      // Save the next node
    head.next = prev;          // Reverse the link
    return reverseList(next, head); // Recursive call with next node and current head as prev
}
```

**Time Complexity:** `O(n)`  
- Each node is visited once.

**Space Complexity:** `O(n)` due to recursion stack depth, but the approach is tail-recursive.

**Advantage:** Tail recursion can be more efficient for certain languages, but JavaScript doesn’t optimize tail recursion extensively.

---

### Summary of Approaches

| Approach               | Time Complexity | Space Complexity | Description                                |
|------------------------|-----------------|------------------|--------------------------------------------|
| Iterative Solution     | `O(n)`         | `O(1)`           | Reverse using three pointers iteratively. |
| Recursive Solution     | `O(n)`         | `O(n)`           | Traverse to the end and reverse links.    |
| Tail Recursive Solution | `O(n)`        | `O(n)`           | Reverse links with tail recursion.        |

The **iterative solution** is typically the most efficient in terms of both time and space for reversing a linked list.