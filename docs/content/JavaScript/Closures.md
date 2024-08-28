https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures

Feature in [[JavaScript]] where an inner function has access to the outer function's variables, even after the outer function has finished executing. This happens because the inner function retains a reference to its outer scope, known as its lexical environment. 


combination of function + references to its surrounding state ([[Lexical Environment]])
- closure gives you access to an other function's scope from an inner function 
- closures are created every time a function is created, at function creation time

How is it created?
- whenever a function is defined within another function(the outer function) and the inner function accesses variables from the outer function's scope 
- when an inner function retains access to its outer function' variables even after the outer function has finished executing
```js
function outerFunction(){
	let outerVariable = "I am from outer funciton";
	
	function innerFunction(){
		console.log(outerVariable);
	}
	return innerFunction;
}

const closure = outerFunction();
closure();
```


Why is this important?
- encapsulation: allow for data encapsulation, private variables and functions
- maintaining state: allow functions to maintain access to variables across multiple calls, retaining state
- creating function factories: useful for creating functions that can share access to the same set of variables 
If closure did not exist: // Error: `count` is not defined in this scope

```js
function createCounter() {
	let count = 0; 

	return function increment(){
		count++; // Error: `count` is not defined in this scope
		console.log(`Coutner value: ${count}`);
	};
}

function asyncIncrement(callback) {
	setTimeout(function(){
		callback();
	}, 1000);
}

// Without closure, `incrementCounter` would just be the `increment` function
const incrementCounter = createCounter();

asyncIncrement(incrementCounter);

```

