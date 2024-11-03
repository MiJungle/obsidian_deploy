### Question
Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` where `nums[i] + nums[j] + nums[k] == 0`, and the indices `i`, `j` and `k` are all distinct.

The output should _not_ contain any duplicate triplets. You may return the output and the triplets in **any order**.

**Example 1:**

```java
Input: nums = [-1,0,1,2,-1,-4]

Output: [[-1,-1,2],[-1,0,1]]
```

Copy

Explanation:  
`nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.`  
`nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.`  
`nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.`  
The distinct triplets are `[-1,0,1]` and `[-1,-1,2]`.

**Example 2:**

```java
Input: nums = [0,1,1]

Output: []
```

Copy

Explanation: The only possible triplet does not sum up to 0.

**Example 3:**

```java
Input: nums = [0,0,0]

Output: [[0,0,0]]
```

Copy

Explanation: The only possible triplet sums up to 0.

**Constraints:**

- `3 <= nums.length <= 1000`
- `-10^5 <= nums[i] <= 10^5`

### Answer
To solve the "Three Sum" problem, where we want to find all unique triplets in an array that sum to zero, we can explore multiple approaches. The most common and optimized solution is a two-pointer approach after sorting the array, which helps avoid duplicate triplets efficiently.

---

### Problem Summary
Given an integer array `nums`, return all unique triplets `[nums[i], nums[j], nums[k]]` such that:
1. `i`, `j`, and `k` are distinct indices.
2. `nums[i] + nums[j] + nums[k] == 0`.

#### Example:
Input:
```plaintext
nums = [-1, 0, 1, 2, -1, -4]
```

Output:
```plaintext
[[-1, -1, 2], [-1, 0, 1]]
```

---

### Approach 1: Sorting + Two-Pointer Technique

1. **Sort the array** to make it easier to avoid duplicates and use two-pointer traversal.
2. **Iterate through the array** with a fixed index `i`. For each `i`, set up two pointers: `left` (starting from `i + 1`) and `right` (starting from the end of the array).
3. Calculate the **sum** of `nums[i] + nums[left] + nums[right]`:
   - If the sum is zero, add the triplet `[nums[i], nums[left], nums[right]]` to the result.
   - If the sum is less than zero, increment `left` to increase the sum.
   - If the sum is greater than zero, decrement `right` to decrease the sum.
4. **Avoid duplicates** by skipping over duplicate values for `i`, `left`, and `right`.

**Time Complexity**: `O(n^2)`  
**Space Complexity**: `O(n)` for the output list.

**Implementation (JavaScript)**:

```javascript
function threeSum(nums) {
    nums.sort((a, b) => a - b); // Step 1: Sort the array
    const result = [];

    for (let i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue; // Skip duplicate values for `i`

        let left = i + 1, right = nums.length - 1;

        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];
            
            if (sum === 0) {
                result.push([nums[i], nums[left], nums[right]]);
                
                // Skip duplicates for `left` and `right`
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;
                
                left++;
                right--;
            } else if (sum < 0) {
                left++;
            } else {
                right--;
            }
        }
    }
    return result;
}
```

### Explanation of Each Step:

- **Sorting** ensures that duplicates are next to each other, which helps in skipping duplicate values easily.
- **Two-pointer method**: By adjusting `left` and `right` based on the sum, we efficiently find triplets without needing nested loops for every combination.
- **Skip duplicates**: After finding a valid triplet, increment `left` and decrement `right` until they point to new values to avoid adding duplicate triplets to the result.

---

### Approach 2: Hash Set Approach (Alternative for Understanding)

This approach uses a hash set to find complement pairs but is less efficient than the two-pointer approach in terms of time complexity.

1. **Fix one element** `i` and find pairs `j` and `k` such that `nums[j] + nums[k] = -nums[i]`.
2. For each `i`, use a set to keep track of elements we've seen so far in the inner loop.
3. If `-target - nums[j]` exists in the set, then `nums[i]`, `nums[j]`, and `-target` form a triplet.

**Time Complexity**: `O(n^2)`, but with higher constant factors than the two-pointer approach.

---

### Summary

| Approach           | Time Complexity | Space Complexity | Description                             |
|--------------------|-----------------|------------------|-----------------------------------------|
| Sorting + Two-Pointer | `O(n^2)`       | `O(n)`          | Most efficient, avoids duplicates easily |
| Hash Set Approach  | `O(n^2)`       | `O(n)`          | Alternative, less efficient             |

### Preferred Solution:
The **Sorting + Two-Pointer Solution** is optimal for this problem because it combines efficient search with easy handling of duplicates, making it both faster and simpler than other methods.
