### State as a Snapshot

Setting state triggers renders
- for an interface to react to the event, you need to update the state

Rendering takes a snapshot in time
- rendering means React is calling your component, which is a function 
- JSX you return from that function is like a snapshot of the UI in time
- props, event handlers, local variables were all calculated using its state at the time of the render 
- when React re-renders a component
	- 1) React calls your function again
	- 2) Your function returns a new JSX snapshot
	- 3) React then updates the screen to match the snapshot your function returned
- **state lives in React itself** , like on a shelf outside of your function 
- **a state variable's value never changes within a render**
- ex) number only increments once per click
	- setting state only changes it for the next render 
	- during the first render, number was 0 
		- so if it is called 3 times , it is 0 called 3 times 
	- setNumber(number +1) -> react prepares to change number to 1 on the next render
	- even with timer like setTimeout, it will still be 0 

```js
import { useState }m from 'react';

export default function Counter() {
	const [number, setNumber] = useState(0);

	return (
		<>
			<h1>{number}</h1>
			<button onClick={()=>{
				setNumber(number + 1);
				setNumber(number + 1);
				setNumber(number + 1);
			}}> +3 </button>
		</>
	)
}


```


### Queueing a Series of State Updates
- Setting a state variable will queue another render, what if you want to perform multiple operations on the value before queueing the next render? 
- **React waits until all code in the event handlers has run before processing your state updates**
- Batching : UI won't be updated until after your even handler and any code in it completes
	- React does not batch across multiple intentional events like clicks - each click is handled separately 


Updating the same state multiple times before the next render
- if you would like to update the same state variable multiple time before the next render, you can pass a function that calculates the next state based on the previous on in the queue like setNumber(n=>n+1)
```js
import { useState } from 'react';

export default function Counter() {
	const [number, setNumber] = useState(0);

	return (
		<>
			<h1>{number}</h1>
			<button onClick={() => {
				setNumber(n => n+1); // updater function
				setNumber(n => n+1);
				setNumber(n => n+1);
			}}>+3</button>
		</>
	)
}
```
- How React works
	- setNumber(n=>n+1) is a function. React adds it to a queue
	- setNumber(n=>n+1) is a function. React adds it to a queue
	- setNumber(n=>n+1) is a function. React adds it to a queue
	- React goes through the queue, the previous number state was 0, it passes updater as the n argument, then React takes the return value of your previous updater function and passes it to the next updater as n

```js
return (
	<>
		<h1>{number}</h1>
		<button onClick={()=>{
			setNumber(number+5);
			setNumber(n => n+1);
			setNumber(42);
			}}>Increase the number</button>
	</>
)
```
- How React works
	- setNumber(0 + 5), adds "replace with 5" to its queue
	- setNumber(n => n+1) React adds that function to its queue
	- setNumber(42) Reacts adds "replace with 42" to its queue
	- after event handler completes, React will trigger a re-render


### Updating Objects in State
- create a new object or make a copy of object when you want to update object
- in Javascript : numbers, strings, booleans are built-in primitive values that you cannot make changes 
- treat any JavaScript objects that you put into state as **read-only** 

Local mutation is fine
- mutation is only a problem when you change existing objects that are already in state 
- mutating an object you've just created is okay because no other code referecnes it yet 
```js
const nextPosition = {};
nextPosition.x = e.clientX;
nextPosition.y = e.clientY;
setPosition(nextPosition);
```

Copying objects with the spread syntax
- shallow : only copies things one level deep, this makes it fast, but it also means that if you want to update a nested property, you will have to use it more than once 
```js
sePerson({
	...person, //copy the old fields
	firstName: e.target.value // but override this one
})

//dynamic name property
function handleChange(e) {
	setPerson({
		...person,
		[e.target.name]: e.target.value
		})
}

```

Updating a nested object
```js
const nextArtwork = { ...person.artwork, city: "New Delhi" };
const nextPerson = {...person, artwork: nextArtwork };
setPerson(nextPerson);

//or
setPerson({
	...person,
	artwork: {
		...person.artwork,
		city: "New Delhi"
	}
})
```

Objects are not nested
```js
let obj = {
	name: "Niki de Saint Phalle",
	artwork: {
		title: "Blue Nana",
		city: "Hamburg",
		image: "https://img.com/sdig.jpb",
	}
}
```
- you are really looking at two different objects:
- obj1 is not "inside" obj2, obj3 can point at obj1 too
- this means that if you were to mutate obj3.artwork.city, it could affect both obj2.artwork.city and obj1.city because they are the same object 
- think of them as separate objects "pointing" at each other with properties
```js
let obj1 = {
		title: "Blue Nana",
		city: "Hamburg",
		image: "https://img.com/sdig.jpb",
}
let obj2 = {
	name: "Niki de Saint Phalle",
	artwork: obj1
}

let obj3 = {
	name: "Copycat",
	artwork: obj1
}
```



Updating Arrays in State
- Arrays are mutable in JavaScript 
- but treat them as immutable when you store them in state 
- create new array and then set state to use the new array
- avoid reassigning items inside an array like arr[0] or using methods that mutate the array like push(), pop() 
- non-mutating methods: filter(), map() 
- ![[Screenshot 2024-06-04 at 9.04.01 AM.png]]

```js
setArtists(
[
	...artists,
	{ id: nextId++, name: name}
]
)

//prepend
setArtist(
[
	{ id: nextId++, name: name},
	...artists
]

)
```

- removing from an array : use filter
```js
import { useState } from 'react';

let initialArtists = [
	{ id: 0, name: "Marta" },
	{ id: 1, name: "Lamidi" },
	{ id: 2, name: "Louise" }
];

export default function List() {
	const [artists, setArtists] = useState(
		initialArtists
	);

	return (
		<>
			<h1>Inspiring sculptors;</h1>
			<ul>
			{artists.map(artist => {
				<li key={artist.id}>
				{artist.name}{' '}
				<button onClick={() => {
					setArtists(
						artists.filter(a=> 
							a.id !== artist.id
						)
					);
				}}>
				Delete
				</button>
			</li>
			})}
		</ul>
	</>
	)
}
```

- transforming an array : use map() 
```js
import { useState } from 'react';

let initialShapes = [
	{ id: 0, type: 'circle', x: 50, y: 100},
	{ id: 1, type: 'circle', x: 150, y: 100},
	{ id: 2, type: 'circle', x: 250, y: 100},
];

export default function shapeEditor() {
	const [shapes, setShapes ] = useState(
		initialShapes
	);

	function handleClick() {
		const nextShapes = shapes.map(shape => {
			if(shape.type === 'square') {
			
				return shape;
			} else {
			
			return {
				...shape,
				y: shape.y + 50,
			};
		}
	});
	setShapes(nextShapes);
	}

	return (
		<>
			<button onClick={handleClick}>
				Move circles down!
			</button>
			{shapes.map(shape => (
			))}
	
	)
}
```
- replacing items in an array : use map
	- receive the item index as the second argument 
	- use it to decide whether to return the original item(the first argument) or something else
```js
import { useState } from 'react';

let initialCounters = [
	0, 0, 0
];

export default function CounterList() {
	const [counters, setCounters] = useState(
		initialCounters
	);

function handleIncrementClick(index){
	const nextCounters = counters.map((c, i) => {
		if(i === index) {
			return c+1;
		} else {
			return c;
		}
	});
	setCounters(nextCounters);
}

return (
	<ul>
		{counters.map((counter, i) => (
			<li key={i}>
				{counter}
				<button onClick={() => {
					handleIncrementClick(i);
				}}>+1</button>
			</li>
		))}
	</ul>
);
}


```

- inserting into an array : use ... with slice()
```js
export default funciton List(){
	const [name, setName] = useState("");
	const [artists, setArtists] = useState(initialArtists);

	function handleClick() {
		const insertAt = 1;
		const nextArtists = [
			...artists.slice(0, insertAt),
			// new item: 
			{ id: nextId++, name: name},
			...artists.slice(insertAt)
		];
		setArtists(nextArtists);
		setName('');
	}


}

```

- making other changes like reverse, sort : copy and then use reverse(), sort()
	- reverse, sort mutates the original array 
	- you can't mutate existing items(objects) inside of it directly
		- copying is shallow, the new array will contain same items as the original one 
		- if you modify an object inside the copied array, you are mutating the existing state , they point to the same object 
		- 
```js
import { useState } from 'react';

const initialList = [
  { id: 0, title: 'Big Bellies' },
  { id: 1, title: 'Lunar Landscape' },
  { id: 2, title: 'Terracotta Army' },
];

export default function List() {
	const [list, setList] = useState(initialList);

	function handleClick() {
		const nextList = [...list];
		nextList.reverse();
		setList(nextList);
	}

	return (
		<>
			<button onClick={handleClick}>
				Reverse
			</button>
			<ul>
				{list.map(artwork => (
					<li key={artwork.id}>{artwork.title}</li>
				))}
			</ul>
		</>
	);
}


///// mutates the existing state
const nextList = [...list];
nextList[0].seen = true;
setList(nextList);
```

- updating objects "inside" arrays
	- they are not really located "inside", each object is a separate value, to which the array "points"
	- when updating nested state, need to create copies from the point where you want to update, and all the way up to the top level 
```js


// creates bugs!
setMyList(){
	const myNextList = [...myList];
	const artwork = myNextList.find(a => a.id === artworkId);
	artwork.seen = nextSeen;
	setMyList(myNextList);
}


// correct 
setMyList(myList.map(artwork => {
	if(artwork.id === artworkId) {
		return {...artwork, seen: nextSeen};
	} else {
		return artwork;
	}
}))


```


### Managing State
- redundant or duplicate state is common source of bugs

Reacting to input with state
- describe the UI you want to see fro the different visual states 
	- ex) initial state, typing state, success state, and trigger the state changes in response to user input 

Choosing the state structure
- shouldn't contain redundant or duplicated information
```js
export default function Form() {
	const [firstName, setFirstName] = useState('');
	const [lastName, setLastName] = useState('');
	const [fullName, setFullName] = useState('');

function handleFirstNameChange(e) {
	setFirstName(e.target.value);
	setFullName(e.target.value + '' + lastName);

}

function handleLastNameChange(e) {
	setLastName(e.target.value);
	setFullName(firstname + ' ' + e.target.value);
}
}

/// Better
export default function Form() {
	const [firstName, setFirstName] = useState('');
	const [lastName, setLastName] = useState('');

	const fullName = firstName + ' ' + lastName;
}


```


Sharing state between components
- remove state from both of them, move it to their closest common parent and then pass it down to them via props : called lifting state up 
```js
export default function Accordion()[
	const [activeIndex, setActiveIndex] = useState(0);
	return(
		<>
		<Panel 
			title="About"
			isActive={activeIndex === 0}
			onShow={() => setActiveIndex(0)}
		>
		With a population of about ....
		</Panel>
		<Panel
			title="Etymology"
			isActive={activeIndex === 1}
			onShow={() => setActiveIndex(1)}
		
		The name comes from ....<span lang="kk-kz">алма</span>
		</Panel>
	</>
	)
]
```

Preserving and resetting state
- when re-render, React needs to decide which parts of the tree to keep and which parts to discard or re-create from scratch 
- React keeps the parts of the tree that "match up" with the previously rendered component tree 
- React lets you override the default behavior, and force a component to reset its state by passing it a different key
	- this tells React that if different component needs to be created from scratch with new data 
```js
return (
	<div>
	<Chat contact={to} />
	</div>
)

/// correct code:
return (
	<div>
	<Chat contact={to} key={to.email} />

)

```


Extracting state logic into a reducer 
- can consolidate all state update logic outside your component in a single function called "reducer"
- you only specify user "actions"

```js
import { useReducer } from 'react';
import AddTask from './AddTask.js';
import TaskList from './TaskList.js';

export default function TaskApp() {
	const [tasks, dispatch] = useReducer(
		taskReducer,
		initialTasks
	);

	function handleAddTask(text) {
		dispatch({
			type: 'added',
			id: nextId++,
			text: text,
		});
	}

	function handleChangeTask(task) {
		dispatch({
			type: 'changed',
			task: task
		});
	}

	funciton handleDeleteTask(taskId) {
		dispatch({
			type: 'deleted',
			id: taskId
		})
	}

	return (
		<>
			<AddTask 
				onAddTask={handleAddTask}
			</>
			<TaskList
				tasks={tasks}
				onChangeTask={handleChangeTask}
				onDeleteTask={handleDeleteTask}
			/>
		</>
	)
}

function tasksReducer(tasks, action) {
	switch(action.type) {
		case 'added': {
			return [...tasks, {
				id: action.id,
				text: action.text,
				done: false
			}];
		}
		case 'changed': {
			return tasks.map(t => {
				if(t.id === action.task.id) {
					return action.task;
				} else {
					return t;
				}
			})
		}
		case 'deleted': {
			return tasks.filter(t => t.id !== action.id);
		}
		default: {
			throw Error('Unknown action: ' + action.type);
		}
	}

let nextId = 3;
const initialTasks = [
	{ id: 0, text: 'Visit Museum', done: true},
	{ id: 1, text: 'Watch Show', done: false},
	{ id: 2, text: 'Lennon Wall', done: false}
]
}

```


Passing data deeply with context
- passing props can become inconvenient if you need to pass some prop through many components or if components need the same information 
- context lets parent component make some information available to any component in the tree below - no matter how deep it is


Scaling up with reducer and context
- reducer : lets you consolidate component's sate update
- context: lets you pass information deep down to other components
- reducer + context : manage state of complex screen 


### Choosing the State Structure

Goal: make state easy to update without introducing mistakes 

Group related state : if you always update 2 or more state variables at the same time, consider merging them 
Avoid contradictions in state : when state is structured in a way that may contradict or disagree with each other, you are leaving room for mistakes
Avoid Redundant state : if you can calculate some information from the existing state variables
Avoid duplication in state
Avoid deeply nested state : deeply hierarchical state is not very convenient, prefer to structure state in flat way 

ex) x,y always change together, so unifying them into a single state variable is better 
```js
const [x, setX] = useState(0);
const [y, setY] = useState(0);

const [position, setPosition] = useState({ x:0, y:0 });
```

Don't mirror props in state
- if parent component passes a different value of messageColor, color state would not be updated , the state is only initialized during the first render 
- use messageColor directly, then it will sync 
- so use initial, or default to clarify that its new values are ignored
```js
function Message({ messageColor }) {
	const [color, setColor] = useState(messageColor);

}
//

function Message({ messageColor }) {
	const [color, setColor] = useState(messageColor);

}
```



Avoid duplication in state
- instead of tree like structure, have each place hold array of child place IDs


### Sharing State Between Components
- lifting state up: if you want the state of two components to always change together, remove state from both of them, move it to their closes common parent, and then pass it down to them via props 
- steps:
	- 1) remove state from the child components
	- 2) pass hardcoded data from the common parent
	- 3) add state to the common parent and pass it down together with the event handlers

- uncontrolled components are easier to use within their parents b/c they require less configuration but less flexible when you want to coordinate them together
- controlled components are maximally flexible but they require the parent components to fully configure them with props 


### Preserving and Resetting State
- react keeps track of which state belongs to which component based on their place int eh UI tree 
- you can control when to preserve state and when to reset it btw re-renders

State is tied to a position in the render tree
- state is held inside React 
- React associates each piece of state it's holding with the correct component by where that component sits in the render tree 
- when React removes a component A, it destroys its state
	- when you render the component A, its states are initialized and added to the DOM
- React sees the tree you return
	- ex) if there is condition like below, to React, these 2 counters have the same address(the first child of the first child of the root)
	```js
	if(...){return <Counter>}
	else {return <Counter>}
```


Different components at the same position reset state 
- resets the state of its entire subtree 
- if you want to preserve the state between re-renders, the structure of your tree needs to "match up" from one render to another 
```js
return (
	<div>
		{isPaused ? (
			<p> See you later! </p>
		) : (
			<Counter />
		)}
....
)
```


Do not nest component function definitions
- every time you click the button, input state disappears
- different MyTextField function is created for every render of MyComponent 
- you're rendering a different component in the same position, so React resets all state below 
- **always declare component functions at the top level** don't nest their definitions
```js
import { useState } from 'react';

export default funciton MyComponent() {
	const [counter, setCounter] = useState(0);

	function MyTextField() {
		const [text, setText] = useState('');
		return(
			<input
				value={text}
				onChange={e => setText(e.target.value)}
			/>
		)
	}
	return (
		<>
			<MytextField />
			<button onClick={() => {
				setCounter(counter +1)}}>
				Clicked {Counter} times </button>
		</>
	)
}
```

Resetting state at the same position
- default: React preserves state of a component while it stays at the same position
- if you want to reset, 
	- 1) render components in different positions
		- each Counter's state gets destroyed each time it's removed from the DOM
	- 2) give each component an explicit identity with key
		- tells React to use the key itself as part of the position, instead of their order within the parent 
		- keys are not globally unique, only specify the position within the parent 
```js


// same component at the same position 
return (
	<div>
		{isPlayerA ? (
			<Counter person="Taylor" />
		) : (
			<Counter person="Sarah" />
		)
		<butotn onClick={() => {
			SetIsPlayerA(!isPlayerA);
		}}>
			Next Player!
			</button>
		</div>
	)
}

// render components in different positions
return (
	<div>
		{isPlayerA && 
			<Counter person="Taylor" />
		}
		{!isPlayerA && 
			<Counter person="Taylor" />
		}
		<button onClick={() => {
			setIsPlayerA(!isPlayerA);
		}}	
		Next player!
		</button>
	</div>
)

// give each component key
return (
	<div>
		{isPlayerA ? (
			<Counter key="Taylor" person="Taylor" />
		) : (
			<Counter key="Sarah" person="Sarah" />
		)}

)

```
![[Screenshot 2024-06-11 at 9.45.21 AM.png]]

Preserving state for removed components
- render all chats, but hide all the others with CSS, the components are not removed from tree, their local state would be preserved
	- this can get very slow if hidden trees are large and contain a lot of DOM nodes
- lift state up and hold the pending message from each recipient in the present component, when the child components get removed it doesn't matter because it's the parent that keeps the important information 
	- this is most common solution
- use different source in addition to React state 
	- localStorage

### Extracting State Logic into a Reducer
- can consolidate all the state update logic outside your component in a single function called reducer

Consolidate state logic with a reducer
- without reducer you will have to use different event handlers :
```js
export default funciton TaskApp() {
	const [tasks, setTasks] = useStaet(initialTasks);

	function handleAddTask(text) {
		setTasks([
			...tasks,
			{
				id: nextId++,
				text: text,
				done: false,
			}
		])
	}

	function handleChangeTask(task) {
		setTasks(
			tasks.map((t) => {
				if(t.id === task.id){
					return task;
				} else {
					return t;
				}
			})
		)
	}

	function handleDeleteTask(taskId) {
		setTasks(tasks.filter((t) => t.id !== taskId));
	}

	return (
		<>
			<h1>Prague itinerary</h1>
			<AddTask onAddTask={handleAddtask}/>
			<TaskList
				tasks={tasks}
				onChangeTask={handleChangeTask}
				onDeleteTask={handleDeleteTask}
			/>
		</>
	)
```

Migrate from useState to useReducer :
- 1) Move from setting state to dispatching actions
	- instead of telling React "what to do", specify "what the user did" by dispatching "actions"
	- the object you pass to dispatch is called an "action"
		- convention to give string type that describes what happened and pass any additional information in other fields 
		- ideally describe "what the user did" rather than "how you want the state to change"
- 2) Write a reducer function
	- 2 arguments : current state and action object, and returns the next state
	- use switch statements as it can be easier to read
- 3) Use the reducer from your component
	- 2 arguments: reducer function and initial state
	- returns : stateful value, a dispatch function 

```js
function handleAddTask(text) {
	dispatch(
	//"action" object:
	{
		type: 'added',
		id: nextId++,
		text: text
	});
}

function handleChangeTask(task) {
	dispatch({
		type: 'changed',
		task: task,
	})
}

//write a reducer function 
function yourReducer(state, action){
}

function tasksReducer(tasks, action){
	if(action.type === "added"){
		return [
			...tasks,
			{
				id: action.id,
				text: action.text,
				done: false
			},
		]
	} else if(action.type === "changed"){
		return tasks.map((t) => {
			if(t.id === action.task.id){
				return action.task;
			} else {
				return t;
			}
		})
	} else if (action.type === "deleted"){
		return tasks.filter((t) => t.id !== action.id);
	} else {
		throw Error("Unknown action: ' + action.type);
	}
}

// using switch statement
function taskReducer(tasks, action) {
	switch(action.type) {
		case "added": {
			return [
				...tasks,
				{
					id: action.id,
					text: action.text,
					done: false,
				}
			]
		}
		case "changed": {
			return tasks.map((t) => {
				if(t.id === action.task.id){
					return action.task;
				} else {
					return t;
				}
			})
		}
		case "deleted": {
			return tasks.filter((t) => t.id !== action.id);
		}
		default: {
			throw Error("Unknown action: " + action.type);
		}
	}

}

// use the reducer from your component
import { useReducer } from 'react';

// from :
const [tasks, setTasks] = useState(initialTasks);
// to :
const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
```


Why are reducers called this way?
- named after reduce() : lets you take an array and "accumulate" a single value out of many 
```js
const arr = [1,2,3,4,5];
const sum = arr.reduce(
	(result, number) => result + number
)
```

useState vs useReducer
- code size: 
	- useState writes less code
	- reducer can help cut down on the code if many event handlers modify state in a similar way
- readability: 
	- useState can be easier to read when state updates are simple 
	- useReducer lets you separate how from what happened of event handlers
- debugging: 
	- useState - can be difficult to tell where the state was set incorrectly and why
	- useReducer: can add console log to see every state update and why it happened(action)
- Testing:
	- reducer: pure function that doesn't depend on component, can export and test separately


Writing reducers well
- reducers must be pure
	- same inputs always result in the same output
	- should not send requests, schedule timeouts, perform any side effects(operations that impact things outside the component)
	- update objects and arrays without mutations
- each action describes a single user interaction, even if that leads to multiple changes in the data
	- ex) user presses "Reset" - > dispatch one reset_form action rather than five separate set-field actions

### Passing Data Deep
- passing props can become verbose and inconvenient if you have to pass them through many components in the middle or if many components in your app need the same informaiton
- context lets the parent component make some information available to any component in the tree below it 

The problem with passing props 
- lifting state up can lead to situation called prop drilling
- what if child needs data like - level of its closest Section - require some way for child to ask for data from somewhere above in the tree -> context 

Steps:
- 1) Create a context
- 2) Use the context from the component that needs the data
	- can only call a hook immediately inside a React component(not inside loops or conditions)
- 3) Provide that context from the component that specifies that data
	- figures out its heading level by asking closest Section above
	- the only way to override some context coming from above is to wrap children into a context provider with a different value
	- different Rect contexts don't override each other 


```js
import { createContext } from 'react';

// only argument : default value
export const LevelContext = createContext(1);


// use the context
// Heading component wants to read LevelContext
export default function Heading({ children }) {
	const level = useContext(LevelContext);
}


// if any components inside this <Section> asks for LevelContext, give them this level 

import { LevelContext } from './LevelContext.js';

export default function Section({ level, children }){
	return (
		<section className="section">
			<LevelContext.Provider value={level}>
				{children}
			</LevelContext.Provider>
		</section>
	);

}
```

Before you use context
- just because you need to pass some props several levels deep doesn't mean you should put that information into context
- start by passing props > extract components and pass JSX as children 
```js
//Before
<Layout posts={posts} />
//After
<Layout><Posts posts={posts}/></Layout>
```

Use cases
- theming
- current account : currently logged in user 
- routing
- managing state : use reducer together with context to manage complex state and pass it down to distant component

### Scaling Up with Reducer and Context 
- reducer: lets you consolidate a component's state update logic
- context: let you pass information deep down to other components 
- reducer + context : manage state of complex screen

Combining a reducer with context
- you may have to explicitly pass down the current state and event handlers that change it as props 
- steps: 
	- 1) Create the context
		- create 2 contexts : 1) provides states 2) provides function that lets components dispatch actions
	- 2) Put state and dispatch into context
	- 3) Use context anywhere in the tree

```js
// create 2 contexts

import { createContext } from 'react';

export const TasksContext = createContext(null);
export const TasksDispatchContext = createContext(null);

//provide them to tree below 

import { TasksContext, TasksDispatchContext } from './TasksContext.js';

export default function TaskApp() {
	const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

	return (
		<TasksContext.Provider value={tasks}>
			<TasksDispatchContext.Provider value={dispatch}>
				...
			</TasksDispatchContext.Provider>
		</TasksContext.Provider>
	);
}

// use tasks and dispatch 

import { TasksContext, TasksDispatchContext } form './TasksContext.js';

export default function TaskList() {
	const tasks = useContext(TasksContext);
	return (
		<ul>
			{tasks.map(task => (
				<li key={task.id}>
					<Task task={task} />
				</li>
			))}
		</ul>
	)
}
```

Putting Reducer and Context in a file 
- it will manage the state with a reducer
- it will provide both contexts to components below
- it will take children as a prop so you can pass JSX to it
	- functions like useTasks, useTasksDispatch are called Custom hooks
	- 

```js
import { createContext, useReducer } from 'react';

export const TasksContext = createContext(null);
export const TasksDispatchContext = createContext(null);

export function TasksProvider({ children }) {
	const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

	return(
		<TasksContext.Provider value={tasks}>
			<TasksDispatchContext.Provider value={dispatch}>
				{children}
			</TasksDispatchContext.Provider>
		</TasksContext.Provider>
	)

function tasksReducer(tasks, action) {
	switch(action.type) {
		case 'added': {
			return [...tasks, {
				id: action.id,
				text: action.text,
				done: false
			}]
		}
		case 'changed': {
			return tasks.map(t => {
				if(t.id === action.task.id){
					return action.task;
				} else {
					return t;
				}
			})
		}
		case 'deleted': {
			return tasks.filter( t => t.id !== action.id);
		}
		default: {
			throw Error('Unknown action: ' + action.type);
		}
	}
}

const initialTasks = [
  { id: 0, text: 'Philosopher’s Path', done: true },
  { id: 1, text: 'Visit the temple', done: false },
  { id: 2, text: 'Drink matcha', done: false }
];

// can also export functions
export function useTasks() {
	return useContext(TasksContext);
}

export function useTasksDispatch() {
	return useContext(TasksDispatchContext);
}

// example usage
const tasks = useTasks();
const dispatch = useTasksDispatch();

```


## Escape Hatches
how to step outside React and connect to external systems

### Referencing Values with refs
- when you want a component to "remember" some information but don't want that information to trigger new renders 
- consider it as a secret pocket of your component that React doesn't track 
	- ex) timeout IDs, DOM elements, other objects that don't impact the component's rendering output 
```js
import { useRef } from 'react';

export default function Counter(){
	let ref = useRef(0);

	function handleClick() {
		ref.current = ref.currnet + 1;
		alert('You clicked ' + ref.curent + ' times!');
	}

	return (
		<button onClick={handleClick}>
			Click me!
		</button>
	);
}
```


### Manipulating the DOM with refs
- there is no built-in way to do things like - accessing the DOM elements managed by React - ex) focus a node, scroll to it, measure its size and position 
```js
import { useRef } from 'react';

export default function Form() {
	const useRef = useRef(null);

	function handleClick() {
		inputRef.current.focus()'
	}

	return (
		<>
			<input ref={inputRef} />
			<button onClick={handleClick}>
				Focus the input
			</button>
		</>
	)
}

```


### Synchronizing with Effects
- lets you run som me code after rendering 
- use them to synchronize your component with a system outside of React
```js
import { useState, useRef, useEffect} from 'react';

function VideoPlayer({ src, isPlaying }){
	const ref = useRef(null);

	useEffect(() => {
		if(isPlaying) {
			ref.current.play();
		} else {
			ref.current.pause();
		}
	}, [isPlaying]);

return <video ref={ref} src={src} loop playsInline />;
}

export default function App(){
	const [isPlaying, setIsPlaying] = useState(false);
	return (
		<>
			<button onClick={() => setIsPlaying(!isPlaying)}>
				{isPlaying ? 'Pause' : 'Play'}
			</button>
			<VideoPlayer
				isPlaying={isPlaing}
				src="https://interactive-examples.com"
			/>
		</>
	)
}


```

### You Might Not Need an Effect
- you don't need effects to transform data for rendering
- you don't need effects to handle user events


### Lifecycle of Reactive Effects
- effects have a different lifecycle from components
- components can : mount, update, unmount
- effects can: start synchronizing something, stop synchronizing 
	- this cycle can happen multiple times if your Effect depends on props and state that change over time 

### Separating Events from Effects
- all code inside Effects is reactive
	- re-synchronize if any of the values they read, like props or state are different than during last render
	- will run again if some reactive value it reads has changed due to a re-render 

### Removing Effect Dependencies
- unnecessary dependencies cause your Effect to run too often, or even create an infinite loop 

### Reusing Logic with Custom Hooks
- ex) fetch data, keep track of whether the user is online, connect to a chat room 


### Referencing Values with Refs
- intentionally mutable: can both read and write 
- secret pocket that React doesn't track
- ref is a plain JavaScript object with the current property that you can read and modify
	- setting state re-renders a component, but changing a ref does not re-render components
```js
import { useRef } from 'react';
const ref = useRef(0);

// useRef returns an object like this 
{
	current: 0 // value you passed to useRef
}
```

- when to use state and when to use ref?
	- when a piece of information is used for rendering: state
	- when a piece of information is only needed by event handlers and changing it doesn't require a re-render : ref

![[Screenshot 2024-06-18 at 8.56.36 AM.png]]
ex) needs to be re-rendered so you can see button text change 
```js
import { useState } from 'react';

export default function Counter() {
	const [count, setCOunt] = useState(0);

	function handleClick(){
		setCount(count + 1);
	}
return (
	<button onClick={handleClick}>
		You clicked {count} times
	</button>
	)
}

// with ref - button text will not change 

import { useRef } from 'react';

export default function Counter() {
	let countRef = useRef(0);

	function handleClick(){
		counterRef.current = countRef.current + 1;
	}

	return(
		<button onClick={handleClick}>
			You clicked {countRef.current} times
		</button>
	)
}


```


### How does useRef work inside?
- useRef could be implemented on top of useState, like below:
- state setter is unnecessary b/c useRef always needs to return the same object 
```js
function useRef(initialValue){
	const [ref, unused] = useState({ current: initialValue });
	return ref;
}
```

When to use Refs
- when you need to "step outside" React and communicate with external APIs
- often a browser API that won't impact the appearance of component
	- storing timeout IDs
	- storing and manipulating DOM elements
	- storing other objects that aren't necessary to calculate the JSX

Best Practices for refs
- treat refs as an escape hatch, if much of application logic relies on refs, rethink your approach
- don't read or write ref.current during rendering 
	- if some information is needed during rendering use state 



### Manipulating the DOM with Refs
- you need access to the DOm elements 
	- ex) focus a node, scroll to it, measure its size and position 
	- ref={inputRef} tells React to put this input's DOM node into inputRef.current 
```js
import { useRef } from 'react';

export default function Form() {
	const inputRef = useRef(null);

	function handleClick(){
		inputRef.current.focus();
	}

	return(
		<>
			<input ref={inputRef}/>
			<button onClick={handleClick}>
			Focus the input
			</button>
		</>
	)
}
```

How to manage a list of refs
- below example won't work as Hooks must only be called at the top-level 
- solution: pass a function to the ref attribute, attribute callback
	- React will call your ref callback with the DOM node when it's time to set the ref, and with null when it's time to clear it 
```js
<ul>
	{items.map((item) => {
		const ref f= useRef(null);
		return <li ref={ref}/>;
	})}
// solution: pass a funciton ot the ref attribute 
// pass a function to the ref attribute 

const itemsRef = useRef(null);

....
<ul>
	{catList.map((cat) => (
		<li
			key={cat}
			ref={(node) => {
				const map = getMap();
				if(node) {
					map.set(cat,node);
				} else {
					map.delete(cat);
				}
			}}
	
	))}
```


When React attaches the refs
- React, every update is split in 2 phases:
	- during render, React calls your components to figure out what should  be on the screen
	- during commit, React applies changes to the DOM 
- during first render, DOM nodes have not yet been created, so ref.current will be null, and during the rendering of updates, the DOM nodes haven't bene updated yet, so it's too early to read them 
- React sets ref.current during the commit, before updating the DOM, React sets the affected ref.current values to null, after updating the DOM, React immediately sets them to the corresponding DOM nodes 

Why Avoid Changing DOM nodes managed by React
- modifying adding children, removing children can lead to inconsistent visual results or crashes 
	- like it is removed and you are trying to set state 
- if you do modify nodes, modify parts that React has no read to update 


### Synchronizing with Effects
- effects let you run some code after rendering so that you can synchronize your component with some system outside of React

What are Effects and how are they different from event
- 2 types of logic inside React components
	- rendering code : lives at the top level of your component
		- take props and state, transform them, return JSX 
		- must be pure, like formula it should only calculate the result and do nothing else
	- event handlers
		- nested functions inside components 
		- contain side effects caused by user action 
- sometimes these 2 types are not enough
	- effects let you specify side effects that are caused by rendering itself, rather than by a particular event 
	- effects run at the end of a commit after screen updates 

```js
function MyComponent(){
	useEffect(() => {
		// code will run after every render
	})
	return <div />;
}
```
- every time your component renders, React will update the screen and then run the code inside useEffect 
- useEffect "delays" a piece of code from running until that render is reflected on the screen 
- *** In React, rendering should be a pure calculation of JSX and should not contain side effects like modifying the DOM 

Problem of below code: 
- it tries to do something with the DOM node during rendering
- when VideoPlayer is called first time, DOM does not exist yet 
- there isn't DOM node yet to call play() or pause() b/c React doesn't know what DOM to create until you return the JSX 
```js 
// error: cannot read properties of null (reading 'pause')

funciton VideoPlayer({ src, isPlaying }){
	const ref = useRef(null);
	if (isPlaying) {
		ref.current.play();
	} else {
		ref.current.pause();
	}
	return <video ref={ref} src={src} loop playsInline />;

}
export default function App(){
	const [isPlaying, setIsPlaying] = useState(false);
	return (
		<>
			<button onClick={() => setIsPlaying(!isPlaying)}
				{isPlaying ? 'Pause' : 'Play'}
			</button>
			<VideoPlayer 
				isPlaying={isPlaying}
				src="https://examples.com"
			/>
		</>
	)
}
```

Solution
- wrap the side effect with useEffect
- you are letting React to update the screen first, then your Effect runs 
- order : 
	- React will update the screen, ensuring the video tag in the DOM with right props 
	- React will then run Effect
	- Effect will call play or pause() depending on the value of isPlaying 
```js
function VideoPlayer({ src, isPlaying }){
	const ref = useRef(null);

	useEffect(() => {
		if(isPlaying){
			ref.current.play();
		} else {
			ref.current.pause();
		}
	})
	return <video ref={ref} src={src} loop playsInline />;
}
```


Effects run after very render, using with useState can create an infinite loop
- effects run as a result of rendering
- setting state triggers rendering
- effect runs -> set state -> re-render 
```js
const [count, setCount] = useState(0);
useEffect(() => {
	setCount(count + 1);=
})
```

Step 2: Specify the Effect dependencies
- if you want to skip unnecessary re-running 
- tells React to skip re-running your Effect if isPlaying is the same as it was during the previous render 
- having multiple dependencies
	- React will only skip re-running the Effect if **all** of the dependencies you specify have exactly same values as they had during the previous render 
	- React compares the dependency values using the Object.is comparison 
```js
useEffect(() => {

},[isPlaying])
```

```js
useEffect(() => {
 // this runs after every render
})

useEffect(() => {
// this runs only on mount (when the component appears)
},[])

useEffect(() => {
 // this runs on mount *and also* if either a or b have changed sinde the las render 
})
```

React allows stable dependencies 
- ref - has stable identity, get the same object from the same useRef call on every render 
- set functions returned by useState also have stable identity 


Step3 : Add Cleanup if needed
- React will call your clean up function each time before the Effect runs again , and one final time when the component unmounts(gets removed)
```js
export default function ChatRoom() {

	useEffect(() => {
	const connection = createConnect();
	connection.connect();
	return () => {
		connection.disconnect();
	}
}, []);
	return <h1> Welcome to the chat! </h1>
}
```


How to handle the Effect firing twice in development
- React intentionally remounts your components in development to find bugs like in the last example
- right question should be "how to run an Effect once" but "how to fix my Effects so that it works after remounting"
- answer: implement cleanup function 

Don't use refs to prevent Effects from firing
- not enough to just make the Effect run once 
```js
const connectionRef = useRef(null);

useEffect(() => {
	if(!connectionRef.current){
		connectionRef.current = createConnection();
		connectionRef.current.connect();
	}
},[]);
```

What are good alternatives to data fetching in Effects
- writing fetch is a very manual approach and has downsides
	- effects don't run on server
	- fetching directly in Effects makes it easy to create network waterfalls
		- render parent component, it fetches data, render child components, it fetches data 
		- this is slower than fetching all data in parallel
	- fetching directly in Effects usually means you don't preload or cache data
	- it's not very ergonomic
		- quite a bit of boilerplate code involved when writing fetch calls in a way that it doesn't suffer from bug like race conditions
- these downsides are not specific to React, it applies to fetching data on mount with any library
- suggestions
	- if you use a framework, use its built-in data fetching mechanism
	- consider using or building a client-side cache 


Effects from each render are isolated from each other

- In Strict Mode, React mounts components twice (in development only!) to stress-test your Effects.

### You Might Not Need an Effect
- effects are an escape hatch that allows
	- step outside of React and synchronize your components with some external system like a non-React widget, network, or browser DOM 
	- if there is no external system involved, you don't need an Effect 
	- removing unnecessary effects will make your code easier to follow

How to remove unnecessary Effects
- don't need Effects to transform data for rendering
	- when you update the state, React will first call component functions to calculate what should be on the screen 
	- React will commit these changes to the DOM, updating screen
	- React will run your Effects 
	- if Effect updates the state, this restarts whole process
- you don't need Effects to handle user events

Updating state based on props or state
- when something can be calculated from the existing props or state, don't put in state, calculate it during rendering 

Caching expensive calculations
- you can cache (or "memoize") an expensive calculation by wrapping it in a useMemo hook
- this tells React that you don't want the inner function to re-run unless either todos or filter have changed 
- React will remember the return value of getFilteredTodos() during the initial render, during the next renders it will check if todos or filter are different 
- function you wrap in useMemo runs during rendering, so this only works for pure calculations 
```js
import { useMemo, useState } from 'react';

function TodoList({ todos, filter }){
	const [newtodo, setNewTodo] = useState('');
	const visibleTodos = useMemo(() => {
		return getfilteredTodos(todos, filter);
	}, [todos, filter]);

}
```


How to tell if a calculation is expensive
- add console log to measure the time spent in a piece of code
- useMemo won't make the first render faster, it helps skip unnecessary work on updates
- test performance : [[CPU Throttling]]
- performance in development might not give accurate results, build app for production and test it on a device like your users have
```js
console.time('filter array');
const visibleTodos = getFilteredTodos(todos, filter);
console.timeEnd('filter array');

//
console.time('filtera array');
const visibleTodos = useMemo(() => {
	return getFilteredTodos(todos, filter);
}, [todos, filter]);
console.timeEnd('fitler array');
```


Adjusting some state when a prop changes
- always check
	- whether you can reset all state with a key
	- calculate everything during rendering 
Sharing logic between event handlers 
- if not sure : ask yourself why this code needs to run 
	- use Effects only for code that should run because the component was displayed to the user
		- ex) something that should appear as page displayed 


[[Race Condition]]: two different requests "raced" against each other and came in a different order than you expected
- ex) when typed "hello", will fetch data with query hello
	- if you type fast, there is no guarantee about which order the responses will arrive in 
	- "hell" response may arrive after "hello" response
- solution: add a cleanup function 
	- it ensures when Effect fetches data, all responses except the last requested on will be ignored 
- other things to consider:
	- caching responses 
	- avoid network waterfalls(so child can fetch data without waiting for every parent)
```js
use Effect(() => {
	let ignore =false;
	fetchResults(query, page).then(json => {
		if(!ignore){
			setResults(json);
		}
	});
	return () => {
		ignore = true;
	};
	}, [query, page]);

function handleNextPageClick() {
	setPage(page + 1);
}
})
```


Recap
- if you can calculate something during render, you don't need an Effect
- to cache expensive calculations, add useMemo, instead of useEffect
- to reset the state of an entire component tree, pass a different key to it
- to reset a particular bit of state in response to a prop change, set it during rendering
- code that runs because a component was displayed should be in Effects, the rest should be in events
- if you need to update the state of several components, it's better to do it during a single event
- whenever you try to synchronize state variables in different components, consider lifting state up
- you can fetch data with Effects, but you need to implement cleanup to avoid race conditions

### Lifecycle of Reactive Effects
- components: 
	- mount : when it's added to the screen
	- update : when it receives new props or state, usually in response to an interaction
	- unmount : when it's removed from the screen 
- effect: to start synchronizing something, ,and later to stop synchronizing 
	- this cycle can happen multiple times if there is dependency 


How React re-synchronizes your Effect
- to stop synchronizing, React will call the cleanup function that your Effect returned after connecting to the "general" room 
- roomId changed general -> travel , so it will start synchronizing 
- user goes to a different screen -> ChatRoom unmounts, there is no need to stay connected, React will stop synchronizing 
- if you write multiple useEffect, it will represent a separate and independent synchronization process
```js
function ChatRoom({ roomId }){
	useEffect(() => {
		const connection = creatConnection(serverUrl, roomId);
		connection.connect();
		return() => {
			connection.disconnect();
		}
	}, [roomId]);
}
```

- check that your Effect represents an independent synchronization process
	- if your Effect doesn't synchronize anything, it might be unncecessary




### Separating Events from Effects
How to choose between event handler and Effect?
- reactive values : props, state, variables
- consider why the code needs to run 
	- event handlers: if need to handle specific interactions
		- logic inside event handlers is not reactive, can read values without "reacting" to their changes
		- ex) sending message should happen when "send" button clicked
	- effects: whenever synchronization is needed
		- logic inside Effects is reactive, re-render causes value to change 
		- ex) keep connected to chat room 


New hook useEffectEvent
- behaves like event handler
	- logic inside is not reactive, and it always "sees" the latest values of your props and state
	- not reactive and must be omitted from dependencies
	- difference: event handlers run in response to a user interactions, whereas Effect Events are triggered by you from Effects 
- only call them from inside Effect
- never pass them to other components or Hooks
```js
import { useEffect, useEffectEvent } from 'react';

function ChatRoom({ roomId, theme }){
	const tonConnected = useEffectEvent(() => {
		showNotification('Connected!', theme);
	})

	useEffect(() => {
		const connection = createConnection(serverUrl, roomId);
		connection.on('connected', () => {
			onConnected();
		});
		connection.connect();
		return () => connection.disconnect();
	}, [roomId]);
}
```

why do you need useEffectEvent
- you have to pass numberOfItems but you don't want to add dependency 
```js
function Page({ url }){
	const { items } = useContext(ShoppingCartContext);
	const numberOfItems = items.length;

	useEffect(() => {
		logVisit(url, numberOfItems);
	}, [url]) //🔴 React Hook useEffect has a missing dependency: 'numberOfItems'

}
```
```js
function Page({ url }){
	const { items } = useContext(ShoppingCartContext);
	const numberOfItems = items.length;

	const onVisit = useEffectEvent(visitedUrl => {
		logVisit(visitedUrl, numberOfItems);
	});

	useEffect(() => {
		onVisit(url);
	}, [url]);

}
```


### Removing Effect Dependencies
- effects "react" to reactive values
	- reactive values?
		- can change due to a re-render 
		- props, variables and functions declared directly inside of your component 


- how to remove a dependency
	- prove to the linter that it doesn't need to be a dependency
	- if you want to change the dependencies, change the surrounding code first 
		- think of dependency list as list of all reactive values used by Effect's code 
	- Do not recommend suppressing linter, as it can lead to intuitive bugs
```js
const roomId = 'music'; // not a reactive value anymore

function ChatRoom() {
	useEffect(() => {
		const connection = createConnection(serverUrl, roomId);
		connection.connect();
		return() => connection.disconnect();
	}, []);
}

}
```


Is your Effect doing several unrelated things?
- if you want to first get data about country, and fetch city info based on the country, don't put it in one Effect do it as below
- 2 separate Effects have 2 separate dependency list, so they won't trigger each other unintentionally 
- if you think this is long, extract them into custom Hook
```js

function ShippingForm({ country }){
	const [cities, setCities] = useState(null);
	useEffect(() => {
		let ignore = false;
		fetch(`/api/cities?country=${country}`)
		.then(response => response.json())
		.then(json => {
			if(!ignore) {
				setCities(json);
			}
		});
		return () => {
			ignore = true;
		}
	}, [country]);

	const [city, setCity] = useState(null);
	const [areas, setAreas] = useState(null);

	useEffect(() => {
		if(city) {
			let ignore = foalse;
			fetch(`api/areas?city=${city}`)
			.then(response => response.json())
			.then(json => {
				if(!ignore){
					setAreas(json);
				}
			});
			return () => {
				ignore = true;
			}
		}
	}, [city]);

}
```

Does some reactive value change unintentionally?
- each newly created object and function is considered distinct from all the others
- doesn't matter that the contents inside of them my be the same 
- objects and functions can make Effect re-synchronize more often than you need
```js
const option1 = { serverUrl : 'https://local.com', roomId: 'music'};
const option2 = { serverUrl : 'https://local.com', roomId: 'music'};
console.log(Object.is(option1, option2)) //false
```
- solution: move static objects and functions outside your component
	- proves linter that it's not reactive
```js
const options = {
	serverUrl : 'https://locahost.com',
	roomId : 'music'
}

function ChatRoom(){
	const [message, setMessage] = useState('');

	useEffect(() => {
		const connection = creatConnection(options);
		connection.connect();
		return() => connection.disconnect();
	}, [])
}
```


### Reusing Logic with Custom Hooks
- creating your own hooks 
- when extract into custom Hooks, the code of your components expresses your intent, not the implementation 

Hook
- name start with "use" 
	- good name means not having to guess about what your custom Hook does, what it takes, and what it returns 
- function that doesn't call hooks don't need to be hooks 
- need to be pure, as code inside custom Hooks will re-run 

ex)
```js
export function useChatRoom({ serverUrl, roomId }){
	useEffect(() => {
		const options = {
			serverUrl: serverUrl,
			roomId: roomId
		};
	const connection = createConenction(options);
	connection.connect();
	connection.on('message', (msg) => {
		showNotification('New message: ' + msg);
	})
	return () => connection.disconnect();
	}, [roomId, serverUrl]);

}
```

Wrapping Effects in custom Hooks is beneficial:
- make the data flow to and from your Effects very explicit
- let your components focus on the intent rather than on the exact implementation of your Effects
- when React adds new features, you can remove those Effects without changing any of your components

Recap
- [[Custom Hooks]] let you share logic between components
- [[Custom Hooks]] must be named starting with `use` followed by a capital letter
- Custom Hooks only share stateful logic not state itself
- you can pass reactive values from one Hoot to another, and they stay up-to-date
- all Hooks re-run every time your component re-renders
- the code of your custom Hooks should be pure, like your component's code
- wrap event handlers received by custom Hooks into Effect events
- don't create custom Hooks like useMount, keep their purpose specific
- It's up to you how and where to choose the boundaries of your code 

