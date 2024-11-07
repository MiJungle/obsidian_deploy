### Question
Given the beginning of a linked list `head`, return `true` if there is a cycle in the linked list. Otherwise, return `false`.

There is a cycle in a linked list if at least one node in the list that can be visited again by following the `next` pointer.

Internally, `index` determines the index of the beginning of the cycle, if it exists. The tail node of the list will set it's `next` pointer to the `index-th` node. If `index = -1`, then the tail node points to `null` and no cycle exists.

**Note:** `index` is **not** given to you as a parameter.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/3ecdbcfc-70fc-429a-4654-cf4f6a7dbe00/public)

```java
Input: head = [1,2,3,4], index = 1

Output: true
```

Copy

Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/89e6716c-9f65-46da-d7b2-f04a93269700/public)

```java
Input: head = [1,2], index = -1

Output: false
```

Copy

**Constraints:**

- `1 <= Length of the list <= 1000`.
- `-1000 <= Node.val <= 1000`
- `index` is `-1` or a valid index in the linked list.


### Solution: Linked List Cycle Detection

**1. Brute Force Solution: Using a `.visited` Flag**  
**Explanation:**  
- Traverse the linked list, marking each node with a `visited` flag.
- If a node is revisited (i.e., its `visited` flag is already `true`), a cycle is detected and we return `true`.
- If no node is revisited, return `false`, indicating no cycle.

**Implementation (JavaScript):**

```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
        this.visited = false;  // Initialize visited flag
    }
}

class Solution {
    /**
     * @param {ListNode} head
     * @return {boolean}
     */
    hasCycle(head) {
        let node = head;

        while (head) {
            // If the current node has already been visited, return true (cycle detected)
            if (head.visited) {
                return true;
            }

            // Mark the current node as visited
            head.visited = true;

            // Move to the next node
            head = head.next;
        }

        // If we reach the end without encountering a visited node, no cycle exists
        return false;
    }
}
```

**Time Complexity:** `O(n)`  
- We traverse the linked list once, where `n` is the number of nodes.

**Space Complexity:** `O(n)`  
- Each node requires additional space for the `visited` flag.

**Drawback:** This approach modifies the linked list by adding a `visited` flag to each node, which might not be allowed or ideal in some scenarios, such as immutable lists.

---

**2. Optimized Solution: Floyd's Tortoise and Hare Algorithm**  
**Explanation:**  
- Use two pointers, slow (tortoise) and fast (hare). The slow pointer moves one step at a time, while the fast pointer moves two steps at a time.
- If there is a cycle, the fast pointer will eventually meet the slow pointer.
- If the fast pointer reaches the end of the list (i.e., `fast === null` or `fast.next === null`), there is no cycle.

**Implementation (JavaScript):**

```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

class Solution {
    /**
     * @param {ListNode} head
     * @return {boolean}
     */
    hasCycle(head) {
        let slow = head;
        let fast = head;

        while (fast && fast.next) {
            slow = slow.next;
            fast = fast.next.next;

            // If the slow pointer and fast pointer meet, a cycle is detected
            if (slow === fast) {
                return true;
            }
        }

        // If we reach the end of the list, there's no cycle
        return false;
    }
}
```

**Time Complexity:** `O(n)`  
- We traverse the list once with two pointers, where `n` is the number of nodes.

**Space Complexity:** `O(1)`  
- We only use two pointers, so the space complexity is constant.

**Advantage:** This solution is more space-efficient since it doesn't require modifying the list or using extra memory for node flags.

---

### Summary of Approaches:

| Approach                     | Time Complexity       | Space Complexity      | Description                           |
|------------------------------|-----------------------|-----------------------|---------------------------------------|
| Brute Force (Visited Flag)   | `O(n)`                | `O(n)`                | Mark nodes with a `visited` flag.    |
| Floyd’s Tortoise and Hare    | `O(n)`                | `O(1)`                | Use two pointers to detect a cycle.  |

**Floyd’s Tortoise and Hare** is generally preferred due to its **O(1) space complexity** and **no need to modify the list structure**, making it the more efficient and scalable solution for cycle detection in linked lists.
