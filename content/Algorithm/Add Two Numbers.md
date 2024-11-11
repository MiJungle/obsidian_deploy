### Question
You are given two **non-empty** linked lists, `l1` and `l2`, where each represents a non-negative integer.

The digits are stored in **reverse order**, e.g. the number 123 is represented as `3 -> 2 -> 1 ->` in the linked list.

Each of the nodes contains a single digit. You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Return the sum of the two numbers as a linked list.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/fee72e19-6a21-45a5-365e-3cb45aba9700/public)

```java
Input: l1 = [1,2,3], l2 = [4,5,6]

Output: [5,7,9]

Explanation: 321 + 654 = 975.
```

Copy

**Example 2:**

```java
Input: l1 = [9], l2 = [9]

Output: [8,1]
```

Copy

**Constraints:**

- `1 <= l1.length, l2.length <= 100`.
- `0 <= Node.val <= 9`


### Solution

**1. Brute Force Solution: Using a Stack**  
**Explanation:**  
- Traverse both linked lists and push the digits to stacks.
- Pop from the stacks to add corresponding digits, keeping track of the carry.
- Construct the result list by adding the digits from the stacks.

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
     * @param {ListNode} l1
     * @param {ListNode} l2
     * @return {ListNode}
     */
    addTwoNumbers(l1, l2) {
        let stack1 = [], stack2 = [];
        let current1 = l1, current2 = l2;

        // Push the digits of l1 and l2 to stacks
        while (current1) {
            stack1.push(current1.val);
            current1 = current1.next;
        }
        while (current2) {
            stack2.push(current2.val);
            current2 = current2.next;
        }

        let carry = 0, dummyHead = new ListNode();
        let current = dummyHead;

        // Pop from the stacks to add corresponding digits
        while (stack1.length > 0 || stack2.length > 0 || carry > 0) {
            let sum = carry;
            if (stack1.length > 0) sum += stack1.pop();
            if (stack2.length > 0) sum += stack2.pop();
            carry = Math.floor(sum / 10);
            current.next = new ListNode(sum % 10);
            current = current.next;
        }

        return dummyHead.next;
    }
}
```

**Time Complexity:** `O(n + m)`  
- `n`: Length of `l1`.
- `m`: Length of `l2`.
- The two linked lists are traversed once, and we perform stack operations which are constant time.

**Space Complexity:** `O(n + m)`  
- The stacks store all the digits from both linked lists.

**Drawback:** This approach uses extra space for the stacks, which might not be optimal for very large numbers.

---

**2. Optimized Solution: Using Iteration (Without Extra Space)**  
**Explanation:**  
- Traverse both linked lists simultaneously.
- Add corresponding digits along with the carry, and create a new node for the result.
- No extra space for stacks is used, and we create the result list in place.

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
     * @param {ListNode} l1
     * @param {ListNode} l2
     * @return {ListNode}
     */
    addTwoNumbers(l1, l2) {
        let dummyHead = new ListNode();
        let current = dummyHead;
        let carry = 0;

        while (l1 || l2 || carry) {
            let sum = carry;
            if (l1) {
                sum += l1.val;
                l1 = l1.next;
            }
            if (l2) {
                sum += l2.val;
                l2 = l2.next;
            }

            carry = Math.floor(sum / 10);
            current.next = new ListNode(sum % 10);
            current = current.next;
        }

        return dummyHead.next;
    }
}
```

**Time Complexity:** `O(n + m)`  
- `n`: Length of `l1`.
- `m`: Length of `l2`.
- We traverse both linked lists once and perform constant-time operations for each node.

**Space Complexity:** `O(max(n, m))`  
- The space complexity is proportional to the length of the resulting linked list, which is `max(n, m)`.

**Advantage:** This solution is more efficient because it does not use extra space for stacks. It directly constructs the result list while iterating through both linked lists.

---

**3. Optimized Space Solution with Single Traversal and Constant Space**  
**Explanation:**  
- This approach is the same as the Iterative solution but includes a focus on optimizing space.
- The result is constructed in-place without extra space for dummy nodes or stacks.

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
     * @param {ListNode} l1
     * @param {ListNode} l2
     * @return {ListNode}
     */
    addTwoNumbers(l1, l2) {
        let carry = 0;
        let head = null;
        let tail = null;

        while (l1 || l2 || carry) {
            let sum = carry;
            if (l1) {
                sum += l1.val;
                l1 = l1.next;
            }
            if (l2) {
                sum += l2.val;
                l2 = l2.next;
            }

            carry = Math.floor(sum / 10);
            const newNode = new ListNode(sum % 10);

            if (!head) {
                head = newNode;
            } else {
                tail.next = newNode;
            }

            tail = newNode;
        }

        return head;
    }
}
```

**Time Complexity:** `O(n + m)`  
- The time complexity remains the same as the iterative approach because each node is visited exactly once.

**Space Complexity:** `O(max(n, m))`  
- Only the resulting list's space is required, which is the sum of the lengths of `l1` and `l2`.

**Advantage:** This solution reduces space usage by directly linking the new nodes and eliminating the need for an extra dummy head node.

---

### Summary of Approaches:

| Approach                    | Time Complexity       | Space Complexity      | Description                        |
|-----------------------------|-----------------------|-----------------------|------------------------------------|
| Brute Force (Using Stacks)  | `O(n + m)`            | `O(n + m)`            | Use stacks to reverse the lists and add the digits. |
| Iterative (Without Extra Space) | `O(n + m)`        | `O(max(n, m))`        | Iteratively add digits and carry, constructing result list in place. |
| Optimized (Single Traversal) | `O(n + m)`           | `O(max(n, m))`        | Directly build the result list without additional space. |

**Iterative Solution** is the most efficient both in terms of time and space, as it directly constructs the result list while iterating through the input lists without requiring extra space for stacks or dummy nodes.