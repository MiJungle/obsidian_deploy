
[[How To Use Async Await in JavaScript]]

### What is Async Await?
Modern JavaScript features that involve promises and asynchronous operations. They allow you to write asynchronous code that looks, behaves like synchronous code. 

- fetchData waits for the 'getDataFromAPI' promise to resolve before logging the result 
- when promise never resolves, the 'await' statement will indefinitely pause execution 



```js 
async function fetchData(){
	const data = await getDataFromAPI(); // Pauses here until the promise resolves
	console.log(data); // this line will never execute if promise never resolves
}

function getDataFromAPI() {
	return new Promise(resolve => {
		setTimeout(() => {
			resolve('Fetched Data');
		}, 1000);
	})
}

fetchData(); // after 1 send, "Fetched Data"

```

```js
async function fetchData(){
	try{
		const response = await fetch('https://');
		if(!response.ok){
			throw new Error("Network response was not ok");
		}
	} catch (error) {
		console.error("Fetch error:", error);
	}

}
```


What does "async" "await" mean to JavaScript engine?
- async: 
	- regardless of what you return inside the function, the function will return a promise 
	- allows result to be awaited 
	- 'async' function start execution synchronously up to the first 'await', after that, they execute asynchronously, allowing the JavaScript engine to handle other tasks while waiting for promises to resolve 
	- prevents blocking the main thread
- await
	- pause execution until a promise settles(resolves or rejects)
	- converts to Promise.resolve(42);
```js
async function getMessage() { return 'Hello'; } getMessage()
// Promise {<fulfilled>: 'Hello'}

async function getValue() {
  const value = await 42; // Converts to Promise.resolve(42)
  console.log(value); // Logs: 42
}
getValue()

// Promise{<fulfilled>: undefined}

async function fetchData() {   
	console.log('Before await');   
	const data = await new 
		Promise(resolve => setTimeout(() => resolve('Hello'), 1000));   
	console.log('After await');   
	return data; 
	}  
console.log('Start'); 
fetchData().then(data => console.log('Fetched:', data)); 
console.log('End');

/ start > Before await > End > After await > Fetched: Hello

```

- what happens step by step 
	- "Start"> function runs synchronously until it encounters await, so it prints "Before await"
	- when function hits the 'await', it **pauses execution of fetchData** until promise is settled 
	- prints "End" and after resolved, function resumes execution from when it stopped 
	- since it is setTimeout, "after await" is printed > then "Fetched: Hello "