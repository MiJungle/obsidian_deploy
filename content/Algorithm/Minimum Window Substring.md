### Question
Given two strings `s` and `t`, return the shortest **substring** of `s` such that every character in `t`, including duplicates, is present in the substring. If such a substring does not exist, return an empty string `""`.

You may assume that the correct output is always unique.

**Example 1:**

```java
Input: s = "OUZODYXAZV", t = "XYZ"

Output: "YXAZ"
```

Copy

Explanation: `"YXAZ"` is the shortest substring that includes `"X"`, `"Y"`, and `"Z"` from string `t`.

**Example 2:**

```java
Input: s = "xyz", t = "xyz"

Output: "xyz"
```

Copy

**Example 3:**

```java
Input: s = "x", t = "xy"

Output: ""
```

Copy

**Constraints:**

- `1 <= s.length <= 1000`
- `1 <= t.length <= 1000`
- `s` and `t` consist of uppercase and lowercase English letters.

### Solution
**1. Sliding Window Solution

**Explanation:**

- The sliding window technique is used to dynamically adjust the range of the substring.
- The idea is to:
    1. Expand the window by moving the right pointer.
    2. Once the window contains all characters of `t`, try to shrink it by moving the left pointer.
    3. Track the minimum length of valid windows.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {string} s
     * @param {string} t
     * @return {string}
     */
    minWindow(s, t) {
        if (s.length < t.length) return "";

        let need = new Map();
        for (let char of t) {
            need.set(char, (need.get(char) || 0) + 1);
        }

        let left = 0, right = 0;
        let have = new Map();
        let validCount = 0;
        let minLen = Infinity;
        let result = "";

        while (right < s.length) {
            // Expand the window by including s[right]
            let char = s[right];
            have.set(char, (have.get(char) || 0) + 1);
            
            // Check if the window contains all characters of t
            if (have.get(char) === need.get(char)) {
                validCount++;
            }

            // Shrink the window from the left if valid
            while (validCount === need.size) {
                let windowLen = right - left + 1;
                if (windowLen < minLen) {
                    minLen = windowLen;
                    result = s.slice(left, right + 1);
                }

                let leftChar = s[left];
                have.set(leftChar, have.get(leftChar) - 1);
                if (have.get(leftChar) < need.get(leftChar)) {
                    validCount--;
                }
                left++;
            }

            right++;
        }

        return result;
    }
}
```

**Time Complexity:** `O(n)`

- We traverse the string `s` once, expanding and contracting the window, and perform constant-time operations on each character.

**Space Complexity:** `O(k)`

- We use two maps (`need` and `have`) to store counts of characters. The space complexity is proportional to the size of the alphabet, `k`, in the worst case (for example, 26 for lowercase letters).

**Advantage:**

- This solution is optimal with linear time complexity because it processes each character of the string `s` at most twice (once while expanding the window and once while shrinking it).

---

**2. Brute Force Solution (Not Recommended for Large Inputs)

**Explanation:**

- This solution checks every possible substring of `s` to see if it contains all the characters of `t`.
- We generate all substrings of `s`, check if they contain all characters of `t`, and keep track of the minimum valid window.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {string} s
     * @param {string} t
     * @return {string}
     */
    minWindow(s, t) {
        let minWindow = "";
        let minLength = Infinity;

        for (let i = 0; i < s.length; i++) {
            for (let j = i + 1; j <= s.length; j++) {
                let substring = s.slice(i, j);
                if (this.containsAllChars(substring, t) && substring.length < minLength) {
                    minWindow = substring;
                    minLength = substring.length;
                }
            }
        }

        return minWindow;
    }

    // Helper function to check if substring contains all characters of t
    containsAllChars(substring, t) {
        let count = new Map();
        for (let char of t) {
            count.set(char, (count.get(char) || 0) + 1);
        }

        for (let char of substring) {
            if (count.has(char)) {
                count.set(char, count.get(char) - 1);
            }
        }

        return [...count.values()].every(val => val <= 0);
    }
}
```

**Time Complexity:** `O(n^2 * k)`

- We generate all substrings of `s` which takes `O(n^2)` time, and for each substring, we check if it contains all characters of `t`, which takes `O(k)` time where `k` is the length of `t`.

**Space Complexity:** `O(k)`

- We use a map to store the count of characters in `t` during the check.

**Drawback:**

- This brute force solution has a high time complexity of `O(n^2 * k)` and is impractical for large strings, making it inefficient for large inputs.

---

**3. Optimized with Hash Map (Same Sliding Window Approach)

**Explanation:**

- This solution is similar to the sliding window approach but uses hash maps to track the character frequencies.
- By using hash maps, we can efficiently track the counts of characters and determine if the current window is valid.

**Implementation (JavaScript):**

```javascript
class Solution {
    /**
     * @param {string} s
     * @param {string} t
     * @return {string}
     */
    minWindow(s, t) {
        if (s.length < t.length) return "";

        let need = new Map();
        let have = new Map();
        let left = 0, right = 0;
        let validCount = 0;
        let minLen = Infinity;
        let result = "";

        // Count characters in t
        for (let char of t) {
            need.set(char, (need.get(char) || 0) + 1);
        }

        while (right < s.length) {
            // Expand the window by including s[right]
            let char = s[right];
            have.set(char, (have.get(char) || 0) + 1);

            // If we have enough of a character, increase the valid count
            if (have.get(char) === need.get(char)) {
                validCount++;
            }

            // Shrink the window from the left if valid
            while (validCount === need.size) {
                let windowLen = right - left + 1;
                if (windowLen < minLen) {
                    minLen = windowLen;
                    result = s.slice(left, right + 1);
                }

                let leftChar = s[left];
                have.set(leftChar, have.get(leftChar) - 1);
                if (have.get(leftChar) < need.get(leftChar)) {
                    validCount--;
                }
                left++;
            }

            right++;
        }

        return result;
    }
}
```

**Time Complexity:** `O(n)`

- We iterate through the string `s` once, and for each character, we perform constant-time operations to check and update counts.

**Space Complexity:** `O(k)`

- We use hash maps to store character frequencies, where `k` is the length of `t`.

---

### Summary of Approaches:

|Approach|Time Complexity|Space Complexity|Description|
|---|---|---|---|
|Sliding Window|`O(n)`|`O(k)`|Optimal approach using sliding window.|
|Brute Force|`O(n^2 * k)`|`O(k)`|Checks all substrings, inefficient for large inputs.|
|Optimized Sliding Window|`O(n)`|`O(k)`|Efficient sliding window with hash maps for frequency counting.|

**Sliding Window** with hash maps is the most efficient solution for this problem, offering optimal time and space complexity. The **brute force solution** is less efficient and not recommended for large inputs.