- lets you add a reducer to your component

```js
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```


```js
import { useReducer } from 'react';

function reducer(state, action) {

}

function MyComponent() {
	const [state, dispatch] = useReducer(reducer, {age: 42});

}
```


#### Parameters
- reducer: reducer function that specifies how the state gets updated 
	- must be pure
	- take state and action as argument
	- return next state
- initialArg: value from which the initial state is calculated 
- init(optional) : initializer function that should return the initial state 


#### Returns
- returns 2 values
	- current state 
	- dispatch function that let you update the state to a different value and trigger a re-render

#### Caveats
- call it top level of your component
- strict mode: React will call your reducer and initializer twice in order to help you find accidental impurities 


`dispatch` function
- parameter: action performed by user
- only updates the state variable for the next render 
- if new value is identical to current state- determined by Object.is comparison, React will skip re-rendering the component and its children 
	- React may still need to call your component before ignoring the result 
- React batches state updates
	- updates screen after all the event handlers have run and have called their set functions 
	- prevents multiple re-renders during a single event 
	- if need to force React to update the screen earlier use `flushSync`
```js
const [state, dispatch] = useReducer(reducer, {age: 42});

function handleClick(){
	dispatch({ type: 'incremented_age' });
}
```


#### Usage
- useReducer returns 2 items
	- current state of this state variable 
	- dispatch function that lets you change it in response to interaction 
```js
import { useReducer } from 'react';

function reducer(state, action){
	if(action.type === "incremented_age"){
	return {
		age: state.age + 1
	};
}
	throw Error("Unknown aciton.")'
}

export default function Counter(){
	const [state, dispatch] = useReducer(reducer, { age: 42 });

	return(
		<>
			<button onClick={() => {
				dispatch({ type: 'incremented_age' })
			}}>
			Increment age
			</button>
			<p> Hello! You are {state.age}.</p>
		</>
	);
	)

}



// reducer
function reducer(state, action) {
	switch(action.type) {
		case 'incremented_age': {
			return {
			name: state.name,
			age: stage.age + 1
		};
	}
	case 'changed_name': {
		return {
			name: action.nextName,
			age: state.age
		};
	}
}
	throw Error('Unknown action: ' + action.type);
}

// don't modify objects, return new objects from your reducer
function reducer(state, action) {
	switch(aciton.type){
		case 'incremented_age': {
			return {
				...state,
				age: state.age + 1
			}
		}
	}

}
```


#### Avoiding recreating the initial state
- pass initial function as 3rd argument, then it will only run during initialization
```js
function createInitialState(username){

}

//have to call createInitialState(username) function on every render
function TodoList({ username }){
	const [state, dispatch] = useReducer(reducer, createInitialState(username));
}

// instead, you can pass it as an initializer function as 3rd argument
function TodoList({ username }){
	const [state, dispatch] = useReducer(reducer, name, createInitialState);
}
```


#### Update objects, arrays instead of mutating them 
- React will use Object.is to determine whether to update
- if you change an object or an array in state directly, React will ignore your update 
```js
function reducer(state, action){
	switch(action.type){
		case "incremented_age" : {
			return {
				...state,
				age: state.age + 1
			}
		}
		case "changed_name" : {
			return {
				...state,
				name: action.nextName
			}
		}
	}

}
```