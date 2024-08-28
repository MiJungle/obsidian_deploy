by Deborah Kurata
https://www.freecodecamp.org/news/immutability-in-javascript-with-examples/?ref=dailydev

Primitives in [[JavaScript]]
- once a primitive value is created, it can't be changed 
- it may seem like you're modifying value, but it is not the case 
```js 
let greet = "Hello";
greet += ", World";
console.log(greet);
```
- it looks like we are changing greet string but JavaScript creates a new string
![[Pasted image 20240116131452.png]]

Arras Mutable?
- arrays are mutable by default 
- variables don't store array, it stores the memory address where the array resides
![[Pasted image 20240116131600.png]]


Mutability provides flexibility - leads to unintended side-effects, especially in larger applications or those involving concurrent operations 


How to Embrace Immutability with Arrays
-> developers often use patterns or methods that do not alter the original array but instead return a new array 

1. Use Spread Operator

```js 
let ages = [42, 22, 35];
ages = [...ages, 8];
console.log(ages);
```
![[Pasted image 20240116131847.png]]


2. Map 
Creates a new array from the existing array, mapping each element using a function we provide, it leaves the original array unchanged.
```js
ages.map(x => x + 1);
```

3. Push -> modifies? YES
Modifies the original array in place, mutating the array. 


4. filter
Creates a new array with items matching the defined criteria. It leaves the original array unchanged.
```js
ages.filter(x => x > 21);
```


5. Sort -> modifies? YES
Sorts the array elements in place, thereby mutating the array.
```js
ages.sort();
```

6. Slice 
Creates a new array from a portion of an existing array. Example copies the original array elements starting at index 1 through index 3 to a new array.
```js 
ages.slice(1, 3);
```

6. Splice -> modifies? YES
 Changes the contents of an array in place, adding, removing, or replacing existing elements. Example code starts replacing elements at index 2, only replaces 1 element, replaces the element with "18".
```js
ages.splice(2, 1, 18);
```

Objects? 
Objects in JavaScript are also mutable by default. We can add or delete properties and change property values "in place" after an object is created.

When a variable is assigned to the object, the variable doesn't store the object, but rather a memory address where the object resides.
```js
let p = {name: "Nee", age: 30};
p.age = 31;
console.log(p);
```


Immutability with Objects

1. Spread 
```js 
let p = {name: "Nee", age: 30};
p = {...p, age: 31};
console.log(p);
```



Why is Immutability Important?
- Once an immutable value is set, it isn't changed. Rather a new value is created. This makes the value predictable and consistent throughout the code. So it aids in managing state throughout the application. 
- Code becomes simpler and less error-prone when data structures don't change unexpectedly
- Fewer side effects & predictable code