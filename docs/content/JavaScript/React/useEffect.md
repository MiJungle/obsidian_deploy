- lets you synchronize a component with an external system 
```js
useEffect(setup, dependencies?)
```

```js
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }){
	const [serverUrl, setServerUrl] = useState('https://local');

	useEffect(() => {
		const connection = createConnection(serverUrl, roomId);
		connection.connect();
		return() => {
			connection.disconnect();
		};
	}, [serverUrl, roomId]);

}
```
- when component is added to the DOM, React will run your setup function 
- after every re-render with changed dependencies, React will first run the clean up function with the old values 
- run your setup function with the new values 
- after component is removed from the DOM, React will run your cleanup function 
- dependencies : list of reactive values
	- reactive values: props, state, variables, functions declared inside body
	- compare using Object.is 
- returns : undefined 

### Usage
- #### Controlling a non-React widget
- #### Fetching data with Effects
- #### Specifying reactive dependencies
- #### Updating state based on previous state from an Effect
- #### Removing unnecessary object dependencies
- #### Removing unnecessary function dependencies
- #### Reading the latest props and state from an Effect
- #### Displaying different content on the server and the client 

### TroubleShooting
- #### My Effect runs twice when the component mounts
- #### My Effect runs after every re-render
- #### My Effect keeps re-running in an infinite cycle
- #### My cleanup logic runs even though my component didn't unmount
- #### My Effect does something visual, and I see a flicker before it runs


#### Caveats
- if not trying to synchronize some external system, might not need useEffect
- in Strict Mode, React will run one extra development-only setup+ cleanup cycle
- remove unnecessary object and function as it may cause Effect to re-run
- if need to block browser from repainting screen, use [[useLayoutEffect]]
- only run on the client, don't run during server rendering 

### Usage
- #### Connecting to an external system
	- in development React runs setup and cleanup one extra time before the setup 
		- setup - cleanup -> setup 
	- if not connecting to external system(like chat), might not need useEffect
		- timer, third party animation with API 

- #### Wrapping Effects in custom Hooks
	- effects are escape hatch
```js
function useChatRoom({ serverUrl, roomId }){
	useEffect(() => {
		const options = {
			serverUrl: serverUrl,
			roomId: roomId
		};
	const connection = createConnection(options);
	connection.connect();
	return () => connection.disconnect();
	}, [roomId, serverUrl]);

}

// usage
function ChatRoom({ roomId }){
	const [serverUrl, setServerUrl] = useState('https://localhost:1234');
	useChatRoom({
		roomId: roomid,
		serverUrl: serverUrl
	})
}
```

- #### Fetching data with Effects
```js
import { useState, useEffect } from 'react';
import { fetchBio } form './api.js';

export default function Page() {
	const [person, setPerson] = useState('Alice');
	const [bio, setBio] = useState(null);

	useEffect(() => {
		let ignore = false;
		setBio(null);
		fetchBio(person).then(result => {
			if(!ignore){
				setBio(result);
			}
		});
		return () => {
			ignore = true;
		}
	}, [person]);


}
```
- downside of writing data fetching directly in Effects 
	- gets repetitive 
	- difficult to add optimizations like caching 
	- effects don't run on server 
		- initial server-rendered HTMl will only include loading state with no data 
		- client computer will have to download all JavaScript and render your app 
		- difficult to add server rendering later
	- create network waterfalls 
		- render parent > fetches data > render child component > fetching data 
		- this process can feel slower than fetching all data in parallel 
	- => solution: 
		- framework 
		- build client-side cache (React Query, useSWR, React Router 6.4+)
		- use a custom hook 



- #### Specifying reactive dependencies
	- reactive values: can be props, variables, functions declared directly inside of your component
	- to remove dependency, prove to the linter that it doesn't need to be dependency
	- suppressing linter makes your code susceptible to bugs, so instead, prove it is not necessary dependency

```js
const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }){
	useEffect(() => {
		const connection = createConnection(serverUrl, roomId);
		connection.connect();
		return () => connection.disconnect();
	}, [roomId]);

}
```

- #### Updating state based on previous state from an Effect
```js
//before relying on count 
// specifying 'count' as dependency always resets the interval
function Counter() {
	const [count, setCount] = useState(0);

	useEffect(() => {
		const intervalId = setInterval(() => {
			setCount(count + 1);
		
		}, 1000)
		return () => clearInterval(intervalId);
	},[count]);
}

// after 

function Counter() {
	const [count, setCount] = useState(0);

	useEffect(() => {
		const intervalId = setInterval(() => {
			setCount(c => c + 1);
		}, 1000);
		return () => clearInterval(intervalId);
	}, [])
}

```


- #### Removing unnecessary object dependencies
	- if Effect depends on an object or function created during rendering, can run too often
	- create the object inside the Effect

```js
// before
const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }){
	const [message, setMessage] = useState("");

	const options = {
		serverUrl: serverUrl,
		roomId: roomId
	};

	useEffect(() => {
		const connection = createConnection(options);
		connection.connect();
		return () => conneciton.disconnect();	
	}, [options]);
}


//after : the Effect only depends on the roomId string
const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }){
	const [message, setMessage] = useState('');

	useEffect(() => {
		const options = {
			serverUrl: serverUrl,
			roomId: roomId
		};
		const connection = createConnection(options);
		connection.connect();
		return () => connection.disconnect();
	}, [roomId]);
}
```

- #### Removing unnecessary function dependencies
	- Effect depending on an object, function it might run too often 
```js
// before
// createOptions function is created from scratch on every re-render
// this itself is not a problem, but when it becomes dependency, it will cause Effect to re-run after every re-render
function ChatRoom({ roomId }){
	const [message, setMessage] = useState("");

	function createOptions(){
		return{
			serverUrl: serverUrl,
			roomId: roomId
		};
	}

	useEffect(() => {
		const options = createOptions();
		const connection = createConnection();
		connection.conenct();
		return () => connection.disconnect();
	}, [createOptions]);
}

// after
function ChatRoom({ roomId }){
	const [message, setMessage] = useState("");

	useEffect(() => {
		function createOptions() {
			return{
				serverUrl: serverUrl,
				roomId: roomId
			};
		}

		const options = createOptions();
		const connection = createConnection(options);
		connection.connect();
		return () => connection.disconnect();	
	}, [roomId]);


}

```

- #### Reading the latest props and state from an Effect
	- [[useEffectEvent]]
```js
//before
function Page({ url, shoppingCart }){
	useEffect(() => {
		logVisit(url, shoppingCart.length);
	}, [url, shoppingCart]);
}
//after
function Page({ url, shoppingCart }){
	const onVisit = useEffectEvent(visitedUrl => {
		logVisit(visitedUrl, shoppingCart.length)
	});

	useEffect(() => {
		onVisit(url);
	}, [url]);
}
```


- #### Displaying different content on the server and the client 
	- effect don't run in server
	- server: will render to produce the initial HTML
	- client: React will run the rendering code again so that can attach your event handlers to that HTML
	- [[Hydration]], in order for hydration to work, initial render output must be identical on the client and server

### TroubleShooting
- #### My Effect runs twice when the component mounts
	- in development, when Strict Mode is on, React runs setup and cleanup one extra time before the actual stop 
		- but to user, there shouldn't be any difference 

- #### My Effect runs after every re-render
	- check dependency 
	- manually log your dependency to the console 

- #### My Effect keeps re-running in an infinite cycle
	- your Effect is updating some state
	- updated state leads to a re-render which causes the Effect's dependencies to change
	- solution: 
		- check effect is necessary - is it synchronizing with external system 

- #### My cleanup logic runs even though my component didn't unmount
	- in development, when Strict Mode is on, React runs setup and cleanup one extra time 
	- cleanup logic should be 'symmetrical' to the setup logic 
```js
useEfect(() => {
	const connection = createConenction(serverUrl, roomId);
	connection.conenct();
	return () => {
		connection.disconnect();
	}
}, [serverUrl, roomId])
```


- #### My Effect does something visual, and I see a flicker before it runs
	- if your Effect must block the browser from painting the screen, replace useEffect with [[useLayoutEffect]]
	- use this only when it's crucial to run your Effect before the browser paint: ex) measure and position a tooltip before user sees it


