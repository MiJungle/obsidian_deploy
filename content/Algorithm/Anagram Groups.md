### Question

Given an array of strings `strs`, group all _anagrams_ together into sublists. You may return the output in **any order**.

An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

**Example 1:**

```java
Input: strs = ["act","pots","tops","cat","stop","hat"]

Output: [["hat"],["act", "cat"],["stop", "pots", "tops"]]
```

Copy

**Example 2:**

```java
Input: strs = ["x"]

Output: [["x"]]
```

Copy

**Example 3:**

```java
Input: strs = [""]

Output: [[""]]
```

Copy

**Constraints:**

- `1 <= strs.length <= 1000`.
- `0 <= strs[i].length <= 100`
- `strs[i]` is made up of lowercase English letters.


### Solution

**1. Brute Force Solution: Pairwise Comparison**  
**Explanation:**  
- Compare each string with every other string to check if they are anagrams.
- Sort the characters of each string and check if the sorted versions are the same.
- If they match, group them together; otherwise, create a new group.

**Implementation (JavaScript):**  

```javascript
function groupAnagrams(strs) {
    const groups = [];
    for (let i = 0; i < strs.length; i++) {
        const anagramGroup = [strs[i]];
        const sortedStr = strs[i].split('').sort().join('');
        for (let j = i + 1; j < strs.length; j++) {
            if (sortedStr === strs[j].split('').sort().join('')) {
                anagramGroup.push(strs[j]);
            }
        }
        groups.push(anagramGroup);
    }
    return groups;
}
```

**Time Complexity:** `O(n^2 * m log m)`  
- `n`: Number of strings in `strs`.
- `m log m`: Sorting each string of length `m` takes `O(m log m)`, and we compare each pair.

**Space Complexity:** `O(n)`  
- Each word and group is stored in the `groups` array.

**Drawback:** This approach is inefficient due to the nested comparison and repeated sorting.

---

**2. Optimized Sorting-Based Solution**  
**Explanation:**  
- Sort each string to create a unique identifier for each anagram group.
- Use a hash map where the sorted string is the key, and values are lists of anagrams.
- For each string in `strs`, sort it, use the sorted result as a key, and append the original string to the corresponding list.

**Implementation (JavaScript):**

```javascript
function groupAnagrams(strs) {
    const map = new Map();
    for (let str of strs) {
        const sortedStr = str.split('').sort().join('');
        if (!map.has(sortedStr)) {
            map.set(sortedStr, []);
        }
        map.get(sortedStr).push(str);
    }
    return Array.from(map.values());
}
```

**Time Complexity:** `O(n * m log m)`  
- `n`: Number of strings.
- `m log m`: Sorting each string takes `O(m log m)`.

**Space Complexity:** `O(n * m)`  
- The hash map stores all strings.

**Advantage:** More efficient than brute force due to using a hash map for grouping.

---

**3. Optimal Character Count-Based Solution**  
**Explanation:**  
- Instead of sorting, create a fixed-size array of 26 elements (for each letter in the alphabet) to represent the character count of each string.
- Convert this array into a unique key by joining the counts, and use it as the hash map key.
- This approach avoids the costly sorting operation, grouping anagrams based on character frequency.

**Implementation (JavaScript):**

```javascript
function groupAnagrams(strs) {
    const map = new Map();
    for (let str of strs) {
        const count = Array(26).fill(0);
        for (let char of str) {
            count[char.charCodeAt(0) - 'a'.charCodeAt(0)]++;
        }
        const key = count.join('#');
        if (!map.has(key)) {
            map.set(key, []);
        }
        map.get(key).push(str);
    }
    return Array.from(map.values());
}
```

**Time Complexity:** `O(n * m)`  
- `n`: Number of strings.
- `m`: Counting each character takes `O(m)`, and no sorting is involved.

**Space Complexity:** `O(n * m)`  
- The hash map stores all strings with unique keys based on character counts.

**Advantage:** This solution is highly efficient for large inputs due to the reduced complexity, avoiding the need to sort each string.

---

### Summary of Approaches:

| Approach                     | Time Complexity       | Space Complexity      | Description                         |
|------------------------------|-----------------------|-----------------------|-------------------------------------|
| Brute Force                  | `O(n^2 * m log m)`    | `O(n)`               | Pairwise comparisons with sorting. |
| Sorting-Based Solution       | `O(n * m log m)`      | `O(n * m)`           | Sort each word, use as hash map key. |
| Character Count-Based Solution | `O(n * m)`           | `O(n * m)`           | Character count array as hash map key. |

The character count solution is the most efficient in terms of time complexity and is typically the best choice for large inputs.