### Question
There are `n` cars traveling to the same destination on a one-lane highway.

You are given two arrays of integers `position` and `speed`, both of length `n`.

- `position[i]` is the position of the `ith car` (in miles)
- `speed[i]` is the speed of the `ith` car (in miles per hour)

The **destination** is at position `target` miles.

A car can **not** pass another car ahead of it. It can only catch up to another car and then drive at the same speed as the car ahead of it.

A **car fleet** is a non-empty set of cars driving at the same position and same speed. A single car is also considered a car fleet.

If a car catches up to a car fleet the moment the fleet reaches the destination, then the car is considered to be part of the fleet.

Return the number of **different car fleets** that will arrive at the destination.

**Example 1:**

```java
Input: target = 10, position = [1,4], speed = [3,2]

Output: 1
```

Explanation: The cars starting at 1 (speed 3) and 4 (speed 2) become a fleet, meeting each other at 10, the destination.

**Example 2:**

```java
Input: target = 10, position = [4,1,0,7], speed = [2,2,1,1]

Output: 3
```

Explanation: The cars starting at 4 and 7 become a fleet at position 10. The cars starting at 1 and 0 never catch up to the car ahead of them. Thus, there are 3 car fleets that will arrive at the destination.

**Constraints:**

- `n == position.length == speed.length`.
- `1 <= n <= 1000`
- `0 < target <= 1000`
- `0 < speed[i] <= 100`
- `0 <= position[i] < target`
- All the values of `position` are **unique**.


### Solution

**1. Stack-Based Solution**

**Explanation:**

- A stack is used to simulate the process of forming car fleets as they move toward the target.
- The cars are sorted by their starting positions in descending order (farthest to closest).
- Each car's time to reach the target is calculated.
- A new fleet is created if a car takes more time than the car ahead of it in the stack.

**Implementation (JavaScript):**

```javascript
function carFleet(target, position, speed) {
    let cars = [];
    for (let i = 0; i < position.length; i++) {
        cars.push([position[i], speed[i]]);
    }

    // Sort cars by starting position in descending order
    cars.sort((a, b) => b[0] - a[0]);

    let stack = [];
    for (let [pos, spd] of cars) {
        let time = (target - pos) / spd; // Time to reach target
        if (stack.length === 0 || time > stack[stack.length - 1]) {
            stack.push(time); // Create a new fleet
        }
    }

    return stack.length; // Number of fleets
}
```

**Time Complexity:** `O(n log n)`

- Sorting the cars by position takes `O(n log n)`.
- Iterating through the cars to compute fleets takes `O(n)`.

**Space Complexity:** `O(n)`

- The stack stores the time to reach the target for each fleet.

**Advantage:**

- This solution efficiently handles the car fleet problem with a clear and logical simulation using a stack.

---

**2. Brute Force Solution (Not Recommended for Large Inputs)**

**Explanation:**

- Calculate the time for every car to reach the target.
- Simulate each car overtaking or forming a new fleet one by one.
- Inefficient for larger datasets due to unnecessary comparisons.

**Implementation (JavaScript):**

```javascript
function carFleet(target, position, speed) {
    let fleets = 0;
    let times = position.map((pos, i) => (target - pos) / speed[i]);

    while (position.length > 0) {
        let leadTime = Math.max(...times); // Lead car's time
        fleets++; // New fleet is formed

        // Remove all cars in the same fleet
        for (let i = 0; i < times.length; i++) {
            if (times[i] <= leadTime) {
                times.splice(i, 1);
                position.splice(i, 1);
                i--;
            }
        }
    }

    return fleets;
}
```

**Time Complexity:** `O(n^2)`

- For each car, potentially iterate through all remaining cars to simulate fleets.

**Space Complexity:** `O(n)`

- Stores times for all cars.

**Drawback:**

- Inefficient and impractical for larger datasets due to nested loops.

---

### Summary of Approaches:

|Approach|Time Complexity|Space Complexity|Description|
|---|---|---|---|
|Stack-Based|`O(n log n)`|`O(n)`|Efficient and logical approach using sorting and simulation.|
|Brute Force|`O(n^2)`|`O(n)`|Inefficient due to nested iterations. Not recommended.|

The **Stack-Based Solution** is the preferred approach, offering optimal time and space complexity for the car fleet problem.