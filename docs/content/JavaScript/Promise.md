https://www.freecodecamp.org/news/javascript-promise-object-explained/
https://hojaleaks.com/javascript-101-understanding-promises-async-await?ref=dailydev

Promise is quite simple once you understand how they work. 

1. How a Promise Works
2. Callbacks vs Promises
3. When to Use Promises Instead of Callbacks
4. Promises and the Fetch API
5. Methods
	1. Promise.all()
	2. Promise.allSettled()
	3. Promise.any()
	4. Promise.race()


1. How a Promise Works
Promise object represents a "pending state"that must be fulfilled at some point later.

ex) You send message to a phone store,
```
A: I want to buy the X phone

B: Hello for contacting us (automated message - similar to receiving Promise object)

C: The X phone is available (Promise is resolved)
```

```
A: I want to buy the X phone

B: Hello for contacting us (automated message - similar to receiving Promise object)

C: The X phone is not available (Promise is rejected)
```

- X Phone is available: similar to resolve() method
- X Phone is not available: similar to reject() method(show that Promise can't be fulfilled because of an error) 

ex) 
```js
let p = new Promise((resolve, reject)=> {
	let is True = true;
	if (isTrue) {
		resolve('Promise resolved'); 
	} else {
	reject('Promise rejected);
	}
})
```


3 States
- Pending : initial state, neither fulfilled nor rejected
- Fulfilled : operation was completely successful
- Rejected : operation failed

When creating a new Promise object, need to pass a callback function that will be called immediately with two arguments, the resolve() and reject() functions 

To handle the Promise object, need to chain the function call with the then() and catch() functions
-> resolve() function corresponds to the then() function, reject() corresponds to the catch() function
ex) 
```js 
let p = new Promise((resolve, reject) => {
	let isTrue = true;
	if (isTrue) {
		resolve('Success')
	} else {
		reject('Error');
	}
});

p
.then(message => console.log(`Promise resolved: ${message}`))
.catch(message => console.log(`Promise rejected: ${message}`));
```

![[Pasted image 20240110095511.png]]

2. Callbacks vs Promises
Promise pattern was created to replace the use of callbacks in certain situations. 
-> When written in promises, it is more intuitive and maintainable

ex) Callback
```js
const isPhoneStore = true;
const isPhoneAvailable = true;

function processMessage(resolveCallback, rejectCallback){
	if(!isPhoneStore){
		rejectCallback({
			name: "Wrong store', 
			message: "Sorry, this is a food store!"
		});
	} else if (!isPhoneAvailable){
		rejectCallback({
			name: "Wrong store', 
			message: "Sorry, this is a food store!"
		});
	} else {
		resolveCallback({
		name: "Ok', 
		message: "The X phone is available! How many you want to buy?"
		});
	}
}
```

ex) Promise
```js
const isPhoneStore = true;
const isPhoneAvailable = true;

function processMessage(){
	return new Promise((resolve, reject) => {
		if(!isPhoneStore){
		rejectCallback({
			name: "Wrong store', 
			message: "Sorry, this is a food store!"
		});
	} else if (!isPhoneAvailable){
		rejectCallback({
			name: "Wrong store', 
			message: "Sorry, this is a food store!"
		});
	} else {
		resolveCallback({
		name: "Ok', 
		message: "The X phone is available! How many you want to buy?"
			});
		}
	});
}

processMessage()
.then(response => console.log(response))
,catch(error => console.log(error));

```


3. When to Use Promises Instead of Callbacks
When you have to wait for certain processes to finish. 
When you use the Fetch API, which is used to run network requests


4. Promises and the Fetch API
Fetch API always returns a Promise object, you need to handle it using then() and catch() method

Promise object is assigned to the response variable in the pending state. If you wait a while and then log the object again, the output will be fulfilled
```js
fetch(`<Your API URL>`)
	.then(response => console.log(response))
	.catch(error => console.log(error));

const response = fetch(`<Your API URL>`);
console.log(response); // Promise {<pending>}

//wait and log the object again
Promise { <fulfilled> : Response }

```

The Fetch API will return a Response object when the promise is fulfilled. That's why usually people name the parameter inside the then() method as response, but fell free to name the response as message, value or anything team agreed on. 


5. Methods
	1. Promise.all()
If one of the promises is rejected, then the method returns the first reject it encounters and stops any further process 
```js
const p1 = Promise.resolve('Success');
const p2 = Promise.resolve(200);
const p3 = Promise.resolve('Finished');

p1.then(message1 => {
	return p2.then(message2 => {
		return p3.then(message3 => {
			return [message1, message2, message3];
			});
		});
}).then(messages => {
	console.log(messages);
})


Promise.all([p1,p2, p3])
.then(messages => console.log(messages))
.catch(error => console.log(error));

```

	2. Promise.allSettled()
Similar to Promise.all() but instead of proceeding to catch(), when one of the promises got rejected, the method will store the reject result and continue processing other promises.

When all promises are settled, the method will return an array of objects that contains the details of each promise.

```js
Promise.allSettled([p1, p2, p3]).then(response => {
	console.log(response);
});

//
[ 
{status: 'rejected', reason: 'Error From Promise One'},
{status: 'fulfilled', reason: 200},
{status: 'fulfilled', reason: 'Finished'}
]
```



	3. Promise.any()
Similar to Promise.all(), except that it returns only a single value from any promise that calls the resolve functions first. 

The first promise is rejected, and once the second promise is resolved, any() method stops any further execution of promises and returns the resolved value
```js
Promise.any([p1, p2, p3]).then(response => {
	console.log(response);
});

//
200
```

	4. Promise.race()
Similar to Promise.any(), with one difference: the promise is settled when any promise is resolved or rejected. 

```js
Promise.race([p1, p2, p3]).then(response => {
	console.log(response);
});

//
Error From Promise One
```
