### Question
You are given two strings `s1` and `s2`.

Return `true` if `s2` contains a permutation of `s1`, or `false` otherwise. That means if a permutation of `s1` exists as a substring of `s2`, then return `true`.

Both strings only contain lowercase letters.

**Example 1:**

```java
Input: s1 = "abc", s2 = "lecabee"

Output: true
```


Explanation: The substring `"cab"` is a permutation of `"abc"` and is present in `"lecabee"`.

**Example 2:**

```java
Input: s1 = "abc", s2 = "lecaabee"

Output: false
```


**Constraints:**

- `1 <= s1.length, s2.length <= 1000`

### Solution

**1. Brute Force Solution: Checking Permutation by Sorting Substrings**  
**Explanation:**

- To determine if `s2` contains a permutation of `s1`, we examine all possible substrings of `s2` that are of the same length as `s1`.
- For each substring, we sort its characters and compare it to the sorted version of `s1`. If any substring matches, we return `true`, indicating that `s2` contains a permutation of `s1`.
- This approach uses nested loops and sorting for comparison.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {string} s1
     * @param {string} s2
     * @return {boolean}
     */
    checkInclusion(s1, s2) {
        // Sort s1 to compare with sorted substrings of s2
        let sorted = s1.split("").sort().join("");

        // Check all substrings of s2
        for (let i = 0; i < s2.length - s1.length + 1; i++) {
            let compare = s2.slice(i, i + s1.length).split("").sort().join("");

            if (sorted === compare) return true;
        }
        return false;
    }
}
```

**Time Complexity:** `O(n * m log m)`

- `n`: Length of `s2`.
- `m`: Length of `s1`.
- For each starting index in `s2`, we extract a substring of length `m` and sort it, which takes `O(m log m)`. This results in a total complexity of `O(n * m log m)`.

**Space Complexity:** `O(m)`

- We store a sorted version of `s1`, which requires `O(m)` space.

**Drawback:**

- Sorting each substring in `s2` is inefficient, especially for longer strings, leading to a relatively high time complexity.

---

**2. Optimized Solution: Sliding Window with Character Count**  
**Explanation:**

- Instead of sorting substrings, we can use a sliding window of length `m` to iterate through `s2`. We compare the frequency of characters in the window with the frequency of characters in `s1`.
- This approach uses two frequency arrays: one for `s1` and one for the sliding window in `s2`. If at any point the two arrays match, we have found a permutation of `s1` in `s2`.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {string} s1
     * @param {string} s2
     * @return {boolean}
     */
    checkInclusion(s1, s2) {
        if (s1.length > s2.length) return false;
        
        // Frequency arrays for s1 and the current window in s2
        let s1Count = Array(26).fill(0);
        let windowCount = Array(26).fill(0);
        
        // Populate the frequency array for s1
        for (let i = 0; i < s1.length; i++) {
            s1Count[s1.charCodeAt(i) - 'a'.charCodeAt(0)]++;
            windowCount[s2.charCodeAt(i) - 'a'.charCodeAt(0)]++;
        }
        
        // Slide the window over s2
        for (let i = s1.length; i < s2.length; i++) {
            if (this.isEqual(s1Count, windowCount)) return true;

            // Add the next character to the window
            windowCount[s2.charCodeAt(i) - 'a'.charCodeAt(0)]++;
            // Remove the first character of the window
            windowCount[s2.charCodeAt(i - s1.length) - 'a'.charCodeAt(0)]--;
        }

        return this.isEqual(s1Count, windowCount);
    }

    // Helper function to compare two frequency arrays
    isEqual(arr1, arr2) {
        for (let i = 0; i < 26; i++) {
            if (arr1[i] !== arr2[i]) return false;
        }
        return true;
    }
}
```

**Time Complexity:** `O(n)`

- `n`: Length of `s2`.
- We process each character in `s2` exactly once and perform constant-time operations to update the window and compare the frequency arrays.

**Space Complexity:** `O(1)`

- The space complexity is constant, as the frequency arrays have a fixed size (26 for lowercase English letters).

**Advantage:**

- This solution is more efficient than the brute force solution as it avoids sorting substrings and only requires linear time to traverse `s2`.

---

**3. Optimized Solution: Sliding Window with HashMap**  
**Explanation:**

- This solution is similar to the sliding window approach but uses a `HashMap` instead of an array to store the character frequencies.
- We update the hashmap as we slide through `s2` and check if the character counts in the current window match those in `s1`.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {string} s1
     * @param {string} s2
     * @return {boolean}
     */
    checkInclusion(s1, s2) {
        if (s1.length > s2.length) return false;

        // Initialize frequency maps for s1 and the window in s2
        let s1Map = new Map();
        let windowMap = new Map();
        
        // Populate the map for s1
        for (let char of s1) {
            s1Map.set(char, (s1Map.get(char) || 0) + 1);
        }
        
        // Initialize the window map for the first window in s2
        for (let i = 0; i < s1.length; i++) {
            windowMap.set(s2[i], (windowMap.get(s2[i]) || 0) + 1);
        }
        
        // Slide the window over s2
        for (let i = s1.length; i < s2.length; i++) {
            if (this.isEqual(s1Map, windowMap)) return true;

            // Add the new character to the window
            windowMap.set(s2[i], (windowMap.get(s2[i]) || 0) + 1);
            // Remove the first character of the window
            windowMap.set(s2[i - s1.length], windowMap.get(s2[i - s1.length]) - 1);
            if (windowMap.get(s2[i - s1.length]) === 0) {
                windowMap.delete(s2[i - s1.length]);
            }
        }

        return this.isEqual(s1Map, windowMap);
    }

    // Helper function to compare two maps
    isEqual(map1, map2) {
        if (map1.size !== map2.size) return false;
        for (let [key, value] of map1) {
            if (map2.get(key) !== value) return false;
        }
        return true;
    }
}
```

**Time Complexity:** `O(n)`

- `n`: Length of `s2`.
- We process each character in `s2` once and perform constant-time operations to update the map and compare the character counts.

**Space Complexity:** `O(k)`

- `k`: The number of unique characters in `s1` and `s2`. In the worst case, `k` is constant (26 for lowercase English letters), so the space complexity is `O(1)`.

**Advantage:**

- This solution provides efficient sliding window with a hash map, offering the same time complexity as the frequency array method but with a more flexible implementation using hash maps.

---

### Summary of Approaches:

|Approach|Time Complexity|Space Complexity|Description|
|---|---|---|---|
|Brute Force (Sorting Substrings)|`O(n * m log m)`|`O(m)`|Sort substrings of `s2` and compare with sorted `s1`.|
|Sliding Window (Frequency Array)|`O(n)`|`O(1)`|Use a sliding window and compare frequency arrays.|
|Sliding Window (HashMap)|`O(n)`|`O(k)` (constant)|Use a sliding window and compare character counts with a hash map.|

**Optimal Solution:** The sliding window approaches (using either frequency arrays or hash maps) are more efficient in terms of both time and space compared to the brute force approach. The sliding window with frequency arrays is particularly optimal for space.