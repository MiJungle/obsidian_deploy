Interface for accessing and manipulating parts of the protocol, such as requests and responses. 

Unlike [[XMLHttpRequest]] - callback-based API, Fetch is promise-based and provides a better alternatives that can be easily used in [[Service Worker]]


### Advantages
- built into modern browsers, no additional libraries are required for basic HTTP requests
- returns a promise that resolves to the 'Response' object

```js
fetch('https://api.example.com')
	.then(response => {
		if(!response.ok) {
			throw new Error('Network response was not ok');
		}
		return response.json(); // parse resopnse body as JSON
	})
	.then(data => {
		console.log(data);
	})
	.catch(error => {
		console.error("There has been a propblem");
	})
```

### Considerations
- [[CORS]] : Cross-Origin Resource Sharing rules apply, so ensure the server allows requests from your origin if fetching data from a different domain 
- Error Handling: 'fetch' only rejects a promise if a network error occurs, it does not automatically reject on HTTP errors(like 404 or 500), you need to handle HTTP errors explicitly 

### Comparison with XMLHttpRequest
- easier to use due to promise-based approach
```js
const xhr = new XMLHttpRequest():
xhr.open('Get', "https://api.example.com", true);
 xhr.onload = function (){
	 if(xhr.status === 200){
		 console.log(JSON.parse(xhr.responseText));
	 } else {
		 console.error("Request failed");
	 }
 };
 xhr.send();

```


### GET, POST 
```js

async function getData(){
	try {
		const response = await fetch("https//json.com");
	}
	if(!response.ok){
		throw new Error("Network response was not ok");
	}
	const data = await response.json();
	console.log("GET data:", data);
	} catch(error) {
		console.error("Error:", error);
	}
}

async function postData() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        title: 'foo',
        body: 'bar',
        userId: 1
      })
    });
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    console.log('POST Data:', data);
  } catch (error) {
    console.error('Error:', error);
  }
}

getData();

```
