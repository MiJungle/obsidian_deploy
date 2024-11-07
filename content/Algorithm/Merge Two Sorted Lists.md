### Question
You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** linked list and return the head of the new sorted linked list.

The new list should be made up of nodes from `list1` and `list2`.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/51adfea9-493a-4abb-ece7-fbb359d1c800/public)

```java
Input: list1 = [1,2,4], list2 = [1,3,5]

Output: [1,1,2,3,4,5]
```

**Example 2:**

```java
Input: list1 = [], list2 = [1,2]

Output: [1,2]
```

**Example 3:**

```java
Input: list1 = [], list2 = []

Output: []
```

**Constraints:**

- `0 <= The length of the each list <= 100`.
- `-100 <= Node.val <= 100`

### Solution

**1. Iterative Solution with Dummy Node**  
**Explanation:**  
- Use a dummy node to simplify the merging process by allowing you to build the list from this temporary node.
- Initialize two pointers (`list1` and `list2`) for each input list.
- Iterate through both lists, comparing nodes and appending the smaller node to the merged list.
- At the end of the loop, append any remaining nodes from `list1` or `list2`.

**Implementation (JavaScript):**

```javascript
function mergeTwoLists(list1, list2) {
    let dummy = new ListNode(-1);  // Dummy node to start the merged list
    let current = dummy;

    while (list1 !== null && list2 !== null) {
        if (list1.val <= list2.val) {
            current.next = list1;
            list1 = list1.next;
        } else {
            current.next = list2;
            list2 = list2.next;
        }
        current = current.next;
    }

    // Append any remaining nodes
    current.next = list1 !== null ? list1 : list2;

    return dummy.next; // Return the merged list starting from the node after dummy
}
```

**Time Complexity:** `O(n + m)`  
- `n`: Number of nodes in `list1`.
- `m`: Number of nodes in `list2`.
  
**Space Complexity:** `O(1)`  
- Only a few pointers are used, so space usage is constant.

**Advantage:** Simple and efficient solution for merging sorted lists.

---

**2. Recursive Solution**  
**Explanation:**  
- Use recursion to compare nodes at the start of `list1` and `list2`.
- Set the smaller node’s `next` to the result of a recursive call for the next node in each list.
- The base case is when either list is empty, in which case the other list is returned.

**Implementation (JavaScript):**

```javascript
function mergeTwoLists(list1, list2) {
    // Base cases
    if (list1 === null) return list2;
    if (list2 === null) return list1;

    // Recursive step
    if (list1.val <= list2.val) {
        list1.next = mergeTwoLists(list1.next, list2);
        return list1;
    } else {
        list2.next = mergeTwoLists(list1, list2.next);
        return list2;
    }
}
```

**Time Complexity:** `O(n + m)`  
- Each node is visited once, so it’s linear in total nodes.

**Space Complexity:** `O(n + m)`  
- The recursion stack uses additional space equal to the total number of nodes.

**Advantage:** Simple and elegant, but it uses extra space due to recursion.

---

**3. In-Place Merge Solution (If modifying input lists is allowed)**  
**Explanation:**  
- Instead of using extra nodes, modify the original lists by directly linking nodes in sorted order.
- Use two pointers to track the current nodes in `list1` and `list2`, updating `next` pointers as you go.
- Use a dummy node for initialization and an extra pointer to build the merged list.

**Implementation (JavaScript):**

```javascript
function mergeTwoLists(list1, list2) {
    if (!list1) return list2;
    if (!list2) return list1;

    let dummy = new ListNode(0);  // Dummy node to start the merged list
    let tail = dummy;

    while (list1 && list2) {
        if (list1.val < list2.val) {
            tail.next = list1;
            list1 = list1.next;
        } else {
            tail.next = list2;
            list2 = list2.next;
        }
        tail = tail.next;
    }

    tail.next = list1 || list2;  // Attach remaining nodes if any

    return dummy.next;
}
```

**Time Complexity:** `O(n + m)`  
**Space Complexity:** `O(1)`  
- Uses constant space since it modifies the list in-place.

**Advantage:** Efficient for large lists as it doesn’t use extra space beyond input lists.

---

### Summary of Approaches

| Approach                | Time Complexity | Space Complexity | Description                          |
|-------------------------|-----------------|------------------|--------------------------------------|
| Iterative with Dummy    | `O(n + m)`     | `O(1)`          | Uses a dummy node to simplify merging |
| Recursive               | `O(n + m)`     | `O(n + m)`      | Recursive merge, but uses extra stack space |
| In-Place Modification   | `O(n + m)`     | `O(1)`          | Modifies input lists directly without extra space |

The **iterative solution with a dummy node** is generally the most efficient and commonly used for merging sorted lists.