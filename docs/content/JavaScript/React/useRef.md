- lets you reference a value that's not needed for rendering 

```js
import { useRef } from 'react';

function MyComponent() {
	const intervalRef = useRef(0);
	const inputRef = useRef(null);

}
```

#### Parameters
- initialValue: value you want ref object's current property to be initially 

#### Returns
- returns an object with a single property
	- current: initially set to the initialValue you passed 

#### Caveats
- mutable, but if it holds an object that is used for rendering, shouldn't mutate that object
- when you change ref.current property, React does not re-render your component 
	- React is not aware of when you change it because ref is a plain JavaScript object
- writing, reading ref.current during rendering can make your component's behavior unpredictable
- strict mode: react will call component twice in order to find accidental impurities


#### Usage
- changing ref does not trigger a re-render
- **refs are perfect for storing information that doesn't affect visual output**
	- use state if you want to store information to display on the screen 
- store information (doesn't reset on every render)
- information is local to each copy of your component 
```js
function handleStartClick() {
	const intervalId = setInterval(() =>{
	
	}, 1000);
	intervalRef.current = intervalId;
}

function handleStopClick() {
	const intervalId = intervalRef.current;
	clearInterval(intervalId);
}
```



```js
import { useRef } from 'react';

export default function Counter() {
	let ref = useRef(0);

	function handleClick(){
		ref.current = ref.current + 1;
		alert('You clicked ' + ref.current + ' times!'); 
	}

	return (
		<button onClick={handleClick}>
			Click me!
		</button>
	)
}
```


#### Manipulating the DOM with a ref
```js
import { useRef } from 'react';

function MyComponent(){
	const inputRef = useRef(null);

function handleClick(){
	inputRef.current.focus();
}

return (
	<input ref={inputRef} />;
)

}
```


#### Avoiding recreating the ref contents
```js
// calling function on every render
function Video(){
	const playRef = useRef(new VideoPlayer());
}

// refactored
function Video(){
	const playerRef = useRef(null);
	if(playerRef.current === null){
		playerRef.current = new VideoPlayer();
	}

}
```