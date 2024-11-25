### Question
You are given an array of strings `tokens` that represents a **valid** arithmetic expression in [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation).

Return the integer that represents the evaluation of the expression.

- The operands may be integers or the results of other operations.
- The operators include `'+'`, `'-'`, `'*'`, and `'/'`.
- Assume that division between integers always truncates toward zero.

**Example 1:**

```java
Input: tokens = ["1","2","+","3","*","4","-"]

Output: 5

Explanation: ((1 + 2) * 3) - 4 = 5
```

**Constraints:**

- `1 <= tokens.length <= 1000`.
- tokens[i] is `"+"`, `"-"`, `"*"`, or `"/"`, or a string representing an integer in the range `[-100, 100]`.


### Solution
**1. Brute Force Solution: Using a Stack

**Explanation:**

- Reverse Polish Notation (RPN) expressions are evaluated using a stack.
- Iterate through each element of the expression:
    - If the element is a number, push it onto the stack.
    - If the element is an operator, pop the top two numbers from the stack, perform the operation, and push the result back onto the stack.
- At the end of the iteration, the stack will contain one element, which is the result of the expression.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {string[]} tokens
     * @return {number}
     */
    evalRPN(tokens) {
        let stack = [];

        for (let token of tokens) {
            if (this.isOperator(token)) {
                let b = stack.pop();
                let a = stack.pop();
                let result = this.applyOperator(a, b, token);
                stack.push(result);
            } else {
                stack.push(Number(token));
            }
        }
        return stack.pop(); // The final result is the only element in the stack
    }

    // Helper function to check if a token is an operator
    isOperator(token) {
        return token === '+' || token === '-' || token === '*' || token === '/';
    }

    // Helper function to apply the operator
    applyOperator(a, b, operator) {
        switch (operator) {
            case '+': return a + b;
            case '-': return a - b;
            case '*': return a * b;
            case '/': return Math.trunc(a / b); // Ensure truncation towards zero for division
        }
    }
}
```

**Time Complexity:** `O(n)`

- `n`: The number of elements in the input array `tokens`. We iterate through each token once and perform constant-time operations for each.

**Space Complexity:** `O(n)`

- The space complexity is `O(n)` because we store the numbers in the stack, where in the worst case, all the elements could be numbers.

---

**2. Optimized Solution: Using Stack (Same Approach with Minor Optimizations)

Since the brute force solution is already quite optimal, the "optimized" solution in this case will focus on a few minor optimizations:

- Avoiding a separate helper function to check if a token is an operator.
- Using the `switch` statement directly in the main loop for clarity.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {string[]} tokens
     * @return {number}
     */
    evalRPN(tokens) {
        let stack = [];

        for (let token of tokens) {
            if (token === '+' || token === '-' || token === '*' || token === '/') {
                let b = stack.pop();
                let a = stack.pop();
                switch (token) {
                    case '+': stack.push(a + b); break;
                    case '-': stack.push(a - b); break;
                    case '*': stack.push(a * b); break;
                    case '/': stack.push(Math.trunc(a / b)); break;
                }
            } else {
                stack.push(Number(token));
            }
        }
        return stack.pop(); // The final result
    }
}
```

**Time Complexity:** `O(n)`

- We process each token exactly once. Each operation inside the loop (push, pop, arithmetic) takes constant time.

**Space Complexity:** `O(n)`

- We use a stack to store operands, and in the worst case, the stack will hold all operands before applying any operations.

---

**3. Optimized Space Solution: In-place Calculation (Not Applicable Here)

For this problem, there isn't really an opportunity for further space optimization beyond using a stack, since each operand needs to be stored temporarily during the evaluation process. Reducing space usage would sacrifice the clarity of the solution.

Thus, the stack-based solution is the most optimal for this problem.

---

### Summary of Approaches:

|Approach|Time Complexity|Space Complexity|Description|
|---|---|---|---|
|**Brute Force (Stack)**|`O(n)`|`O(n)`|Standard approach using a stack for RPN evaluation.|
|**Optimized Solution (Stack)**|`O(n)`|`O(n)`|Simplification of the brute force approach.|
|**In-place Calculation**|Not Applicable|Not Applicable|Not applicable in this problem due to the need for temporary storage.|

### Conclusion:

For this problem, the **stack-based solution** is both time-efficient and space-efficient, with a time complexity of `O(n)` and space complexity of `O(n)`. This approach is optimal and standard for evaluating Reverse Polish Notation expressions.