### Question
Implement the [Least Recently Used (LRU)](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU) cache class `LRUCache`. The class should support the following operations

- `LRUCache(int capacity)` Initialize the LRU cache of size `capacity`.
- `int get(int key)` Return the value cooresponding to the `key` if the `key` exists, otherwise return `-1`.
- `void put(int key, int value)` Update the `value` of the `key` if the `key` exists. Otherwise, add the `key`-`value` pair to the cache. If the introduction of the new pair causes the cache to exceed its capacity, remove the least recently used key.

A key is considered used if a `get` or a `put` operation is called on it.

Ensure that `get` and `put` each run in O(1)O(1) average time complexity.

**Example 1:**

```java
Input:
["LRUCache", [2], "put", [1, 10],  "get", [1], "put", [2, 20], "put", [3, 30], "get", [2], "get", [1]]

Output:
[null, null, 10, null, null, 20, -1]

Explanation:
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 10);  // cache: {1=10}
lRUCache.get(1);      // return 10
lRUCache.put(2, 20);  // cache: {1=10, 2=20}
lRUCache.put(3, 30);  // cache: {2=20, 3=30}, key=1 was evicted
lRUCache.get(2);      // returns 20 
lRUCache.get(1);      // return -1 (not found)
```

Copy

**Constraints:**

- `1 <= capacity <= 100`
- `0 <= key <= 1000`
- `0 <= value <= 1000`

### Solution

**1. Brute Force Solution: Using an Array**  
**Explanation:**  
- We use an array to store keys and values, and whenever we access or add a key, we update its position to the end of the array to mark it as recently used.
- If the cache exceeds the capacity, we remove the first element from the array (least recently used).
- While simple, this solution has a high time complexity for searching and updating keys in the cache.

**Implementation (JavaScript):**

```javascript
class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = [];
    }

    get(key) {
        const index = this.cache.findIndex(item => item[0] === key);
        if (index === -1) return -1;

        const [foundKey, value] = this.cache.splice(index, 1)[0];
        this.cache.push([foundKey, value]);  // Move the accessed key to the end
        return value;
    }

    put(key, value) {
        const index = this.cache.findIndex(item => item[0] === key);
        
        if (index !== -1) this.cache.splice(index, 1);  // Remove the old key if exists
        this.cache.push([key, value]);  // Add new key-value pair

        if (this.cache.length > this.capacity) this.cache.shift();  // Remove the least recently used item
    }
}
```

**Time Complexity:** `O(n)`  
- `O(n)` for `get` and `put` due to the need to find and remove the key in the array.

**Space Complexity:** `O(capacity)`  
- The array stores up to `capacity` key-value pairs.

**Drawback:** This approach is inefficient for larger caches, as it has linear search and update time complexity.

---

**2. Optimized Solution: Using Map**  
**Explanation:**  
- JavaScript’s `Map` keeps keys in the order they were added, making it useful for LRU caching.
- We store key-value pairs in the `Map` and move accessed keys to the end by deleting and re-adding them.
- If the cache exceeds the capacity, we remove the first (least recently used) element from the `Map`.

**Implementation (JavaScript):**

```javascript
class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = new Map();
    }

    get(key) {
        if (!this.cache.has(key)) return -1;

        const value = this.cache.get(key);
        this.cache.delete(key);  // Remove and re-insert to update its position
        this.cache.set(key, value);
        return value;
    }

    put(key, value) {
        if (this.cache.has(key)) this.cache.delete(key);
        
        this.cache.set(key, value);

        if (this.cache.size > this.capacity) {
            const firstKey = this.cache.keys().next().value;
            this.cache.delete(firstKey);  // Remove the least recently used item
        }
    }
}
```

**Time Complexity:** `O(1)`  
- `get` and `put` operations are constant time due to the efficient lookup and ordering in `Map`.

**Space Complexity:** `O(capacity)`  
- The `Map` stores up to `capacity` key-value pairs.

**Advantage:** Efficient `O(1)` access and update time for both `get` and `put` operations.

---

**3. Optimized Solution with Doubly Linked List and Hash Map**  
**Explanation:**  
- This approach uses a doubly linked list for maintaining the order of keys and a hash map for fast access.
- Each node in the linked list represents a cache entry, with pointers to both its previous and next nodes.
- The hash map stores references to the nodes, allowing quick access and removal.
- On access or insertion, the node is moved to the head of the list, marking it as recently used.

**Implementation (JavaScript):**

```javascript
class ListNode {
    constructor(key, value) {
        this.key = key;
        this.value = value;
        this.prev = this.next = null;
    }
}

class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = new Map();
        this.head = new ListNode(0, 0);  // Dummy head
        this.tail = new ListNode(0, 0);  // Dummy tail
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }

    _remove(node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    _add(node) {
        node.next = this.head.next;
        node.prev = this.head;
        this.head.next.prev = node;
        this.head.next = node;
    }

    get(key) {
        if (!this.cache.has(key)) return -1;

        const node = this.cache.get(key);
        this._remove(node);  // Remove node from its current position
        this._add(node);  // Move node to the head (recently used)
        return node.value;
    }

    put(key, value) {
        if (this.cache.has(key)) {
            this._remove(this.cache.get(key));
        }
        const newNode = new ListNode(key, value);
        this._add(newNode);
        this.cache.set(key, newNode);

        if (this.cache.size > this.capacity) {
            const lru = this.tail.prev;
            this._remove(lru);
            this.cache.delete(lru.key);  // Remove the least recently used entry
        }
    }
}
```

**Time Complexity:** `O(1)`  
- Constant time for both `get` and `put` due to efficient hash map lookup and doubly linked list operations.

**Space Complexity:** `O(capacity)`  
- Storage requirement for hash map and linked list nodes, each storing up to `capacity` entries.

**Advantage:** This solution is highly efficient and provides constant-time operations for both access and modification.

---

### Summary of Approaches

| Approach                       | Time Complexity       | Space Complexity      | Description                         |
|--------------------------------|-----------------------|-----------------------|-------------------------------------|
| Brute Force (Using Array)      | `O(n)`               | `O(capacity)`         | Uses array, with linear search and update times. |
| Optimized Solution (Using Map) | `O(1)`               | `O(capacity)`         | Efficient `get` and `put` using JavaScript Map. |
| Optimal Solution (Doubly Linked List + Hash Map) | `O(1)` | `O(capacity)` | Fast access and update with constant-time operations. |

The **Doubly Linked List + Hash Map** solution is the most efficient, providing true constant-time access and update with optimal memory management.