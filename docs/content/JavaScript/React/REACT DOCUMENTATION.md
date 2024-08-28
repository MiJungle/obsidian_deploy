### Using [[TypeScript]] 
- supports JSX
- can get full React Web Support by adding @types/react and @types/react-dom 

@types/react include types for the built-in Hooks, can use them in components without any additional setup 

Example Hooks
- can explicitly provide a type for the state 
```js
const [enabled, setEnabled] = useState<boolean>(false);

// union type
type status = "idle" | "loading" | "success" | "error";
const [status, setStatus] = useState<Stataus>("idle");

// group related state as an object 
type RequestState = 
	| {status: "idle"}
	| {status: "loading"}
	| {status: "success", data: any}
	| {status: "error", error: Error};

const [requestState, setRequestState] = useState<RequestState>({status: "idle"});
```


### React Developer Tools
- install browser extension : React Developer Tool 


### React Compiler
- compiler includes an [[eslint]] plugin, the plugin runs independently of the complier and can be used even if you aren't using compiler 
- What does the compiler do?
	- understands your code at deep level through its understanding of plain JavaScript semantics and the Rules of React 
	- allows it to add automatic optimizations to your code
	- designed to compile functional components and hooks that follow the Rules of React 


### Learning React
### Describing the UI
- React: JavaScript library for rendering user interfaces 
- React lets you combine them into reusable, nestable components

Components all the way down 
- React application begins at a "root" component usually defined in pages/index.js


Importing and Exporting Components
![[Screenshot 2024-05-22 at 9.22.45 AM.png]]


-  for default import, you can put any name and it will work, import Banana from './Button.js' will work 
- but for named import, name has to match - that is why it is called named import 
- a file can only have one default export 

Writing Markup with JSX
- JSX : syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file 
- JSX and React are 2 separate things, so you can use them independently 
- background: as Web became more interactive, logic increasingly determined content, JavaScript was in charge of the HTML 
	- React, rendering logic and markup live together in the same place - components
	- ensures that logic and markup stay in sync with each other on every edit 
- empty tag <> </> is called Fragment, lets you group things without leaving any trace in the browser HTML tree
	- it is same as if the elements were not grouped 


JavaScript in JSX with Curly Braces
- JSX lets you write HTML-like markup inside a JavaScript file 
	- curly braces to use variables, function in html
	- only 2 ways to use curly braces : text & attribute
	- can pass objects
``` js
const today = new Date();

funciton formatDate(date) {
	return new Intl.DateTimeFormat(
		'en-US',
		{weekday: "long"}
		).formate(date);
}

export default function TodoList() {
	return( 
		<h1>To Do List for {formatDate(today)}</h1>
		);
}
```

```js
export default function TodoLIst() {
return (
	<ul style={{
	backgroundColor: "black",
	color: "pink"
	}}>
		<li>Improve the videophone</li>
		<li>Prepare aeronautics lectures</li>
		<li>Work on the alocohol-fuelled engine</li>
	</ul>
)
}
```

```js
const person = {
	name: 'Gregorio Y. Zara',
	theme: {
		backgroundColor: 'black',
		color: 'pink'
	}
};

export default function TodoList() {
	return(
		<div style={person.theme}>
			<h1>{person.name}'s Todos</h1>
			<img
				className="avatar"
				src="https://i.imgur.com/7vad.jpg"
				alt="Gregorio Y. Zara"
				/>
			<ul>
				<li>Imporve the videophone</li>
				<li>Prepare aeronautics lectures</li>
				<li>Work on the alcohol-fuelled engine</li>
			</ul>
		</div>
	);
}
```


### Passing Props to a Component
props: react components use props to communicate with each other 
- pass objects, arrays, functions 
- can specify default value for size
	- **if size prop is missing or undefined, default value will be used but if you pass null or 0, default value will not be used 

```js
export default function Profile() {
	return (
		<Avatar
			person={{ name: 'Lin Lanying', imageId: "lbx5qh6'}}
			size={100}
		/>
	);
}

import {getImageUrl} from './utils.js';

function Avatar({person, size = 100}){
	return(
		<img
			className="avatar"
			src={getImageUrl(person)}
			alt={person.name}
			width={size}
			height={size}
		/>
	);
}
```

can refactor as below : 
```js
function Profile({ person, size, isSepia, thickBorder }){
	return(
		<div className="card">
			<Avatar
				person={person}
				size={size}
				isSepia={isSepia}
				thickBroder={thickborder}
		/>
		</div>
	);
}


function Profile(props){
	return(
		<div className="card">
			<Avatar {...props}/>
		</div>
	);
}
```


### [[Conditional Rendering]]

```js
function Item({ name, isPacked}){
	if (isPacked) {
		return <li className="item">{name}</li>;
	}
	return <li className="item">{name}</li>;
}

export default function PackagingList() {
	return (
		<section>
			<h1>Sally Ride's Packing List </h1>
			<ul>
				<Item
					isPacked={true}
					name="Space suit"
				/>
				<Item
					isPacked={true}
					name="Helmet with a golden leaf"
				/>
				<Item
					isPacked={false}
					name="Photo of Tam"
				/>
			</ul>
		</section>
			)
}

// if you don't want to render anything at all, return null

if(isPacked) {
	return null;
}
return <li className="item">{name}</li>;

```


**Are two examples different? Isn't one of them creating 2 different instances? 
COMPLETELY EQUIVALENT
- JSX elements aren't instances, because they don't hold any internal state and aren't real DOM nodes , they're lightweight descriptions like blueprints 
```js

if (isPacked) {
	return <li className="item">{name} ✔</li>;
	}
return <li className="item">{name}</li>;


return (
	<li className="item">
		{isPakced ? name + '✔' : name}	
	</li>
)
```

Using && 
- *** DON'T PUT NUMBERS ON THE LEFT SIDE OF &&
- To test condition JavaScript converts the left side to a boolean automatically 
- if the left side is 0, whole expression gets value 0 and will render 0 

```js
// if isPacked, render the checkMark, otherwise render nothing
function Item( {name, isPacked }){
	return (
		<li className="item">
			{name} {isPacked && '✔'}
		</li>
	);
}


return (
	<li className="item">
		{isPacked ? name + '✔' : name}
	</li>
)
```


### [[Rendering Lists]]
[[filter()]] , [[map()]]

```js
const people = [
'Creola Katherine Johnson: mathematician',  
'Mario José Molina-Pasquel Henríquez: chemist',  
'Mohammad Abdus Salam: physicist',  
'Percy Lavon Julian: chemist',  
'Subrahmanyan Chandrasekhar: astrophysicist'
]

export default function List() {
	const listItems = people.map(person => 
		<li>{person}</li>
	);
	return <ul>{listItems}</ul>;
}
							
```


```js
const people = [{  
id: 0,  
name: 'Creola Katherine Johnson',  
profession: 'mathematician',}, {  
id: 1,  
name: 'Mario José Molina-Pasquel Henríquez',  
profession: 'chemist',}, {  
id: 2,  
name: 'Mohammad Abdus Salam',  
profession: 'physicist',}, {  
id: 3,  
name: 'Percy Lavon Julian',  
profession: 'chemist',  }, {  
id: 4,  
name: 'Subrahmanyan Chandrasekhar',  
profession: 'astrophysicist',}];

```

```js

export default function List() {

const listItems = chemists.map(person => 
	<li>
		<img 
			src={getImageUrl(person)}
			alt={person.name}
		/>
		<p>
			<b>{person.name}:</b>
			{' ' + person.profession + ' '}
			known for {person.accomplishment}
		</p>
	</li>
);
return <ul>{listItems}</ul>;
}
```

[[filter()]]
```js
import { people } from '/data.js';
import { getImageUrl } from './utils.js';

export default function List() {
	const chemists = people.filter(person =>
		person.profession === 'chemist'
	);
	const listItems = chemists.map(person =>
		<li>
			<img
			src={getImageUrl(person)}
			alt={person.name}
			/>
			<p>
				<b>{person.name}:<b>
				{' ' + person.profession + ' '}
				known for {person.accomplishment}
			</p>
		</li>
	);
	return <ul>{listItems}</ul>;
}
```


*** Arrow functions implicitly return the expression right after =>, so you didn't need a return statement : 
*** Keys tell React which array item each component corresponds to, so that it can match them up later. This becomes important if your array items can move(e.g. due to sorting), get inserted, or get deleted.
 - a well-chosen key helps React infer what exactly has happened and make the correct updates to the DOM tree
 - think of it as files on your desktop, if you refer them by order first file, second file, it gets confusing when file gets deleted, second file becomes first file and so on 
 - using item's index - not recommended, this is used as default if key is not provided, but the order you render items can change if item is inserted, deleted and index as a key leads to subtle and confusing bugs 
 - Math.random() : not recommended as keys will never match up btw renders, leading all your components and DOM being recreated every time 
 - *** components won't receive key as a prop, it's only used as a hint by React itself 
```js
const listItems = chemists.map(person => 
		<li key={person.id}>...</li>
	);

const listItems = chemists.map(person => {
	return <li key={person.id}>...</li>;
})
```


### Keeping Components Pure
- pure functions only perform a calculation and nothing more 
- by writing pure functions, can avoid an entire class of baffling bugs and unpredictable behavior as code grows

Rules
- Purity : Components as Formulas
	- pure function
		- it minds its own business
		- same inputs, same outputs 
	- likewise React assumes that every component you write is a pure function 
	- React components you write must always return the same JSX given the same inputs
- Side Effects : (un)intended consequences
	- components should **ONLY RETURN** their JSX and **NOT CHANGE** any objects or variables that existed before rendering 
- Local Mutations
	- pure functions don't mutate variables outside of the function's scope or objects that were created before the call that makes them impure
	- but it is fine to change variables and objects that you've just created while rendering 
	- cups array is created during the same render, so no code outside of TeaGathering will ever know that this is happened - it is called local mutation

```js
function Cup({ guest }){
	return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaGathering() {
	let cups = [];
	for (let i = 1; i <=12; i++){
		cups.push(<Cup key={i} guest={i} />)};
		return cups;
}
```

Where you can cause side effects
- changes - updating the screen, starting an animation, changing the data - side effects - happen 'on the side' not during rendering
- side effects belong inside event handlers - functions that runs when you perform actions(like clicking a button) - they don't run during rendering 
	- event handlers don't need to be pure 
- as a last resort, you can youse useEffect but try to express logic with rendering alone



### Understanding Your UI as a [[Tree]]

React model UI as a tree
- render tree
- tree is composed of nodes, represents a component 
- root node - App, is the first component React renders
- top-level components are the components nearest to the root component and affect the rendering performance of all the components beneath them and often contain the most complexity 
- leaf components are near the bottom of the tree and have no child components and are often frequently re-rendered 

Trees are relationship model btw items and UI is often represented using tree structures
- ex) browsers use tree structures to model HTML(DOM), CSS(CSSOM)

The Module Dependency Tree
- app's module dependencies can also be modeled with a tree 
- App.js : entrypoint file which contains root component
![[Screenshot 2024-05-24 at 5.01.54 AM.png]]

- there is build step that will bundle all the necessary JavaScript to ship to the client 
	- bundler : will use dependency tree to determine which modules should be included
	- large bundle sizes are expensive for a client to download and run, delay the time for your UI to get drawn 



### Adding Interactivity
- state : data that changes over time 
	- components need to remember : the current input value, the current image, shopping cart 
	- add state to a component with a useState Hook 
	- hooks : special functions that let your components use React features 
		- useState : takes initial state and returns a pair of values : current state and state setter function that lets you update it 
```js
import { useState } from 'react';
import {sculptureList } from './data.js';

export default function Gallery() {
	const [index, setIndex] = useState(0);
	const [showMore, setShowMore] = useState(false);
	const hasNext = index < sculptureList.length - 1;

	function handleNextClick() {
		if(hasNext){
			setIndex(index + 1);
		} else {
			setIndex(0);
		}
	}

	function handleMoreClick() {
		setShowMore(!showMore);
	}

	let sculpture = sculpturesList[index];
	return (
		<>
			<button onClick={handleNextClick}>
			Next
			</button>
			<h2>
				<i>{sculpture.name}</i>
				by {sculpture.artist}
			</h2>
			<h3>
			({index +1} of {sculptureLIst.length})
			</h3>
			<button onClick={handleMoreClick}>
				{showMOre ? 'Hide' : 'Show'} details
			</button>
			<img 
				src={sculpture.url}
				alt={sculpture.alt}
			/>
		</>
	);
}
```

State as a snapshot
- React state behaves more like a snapshot 
- setting it does not change the state variable you already have, but instead triggers a re-render
- setting state requests a new re-render, but does not change it in the already running code 
```js
setScore(score + 1);
console.log(score); // 0 

function increment() {
	setScore(s => s + 1);
}

```

### Responding to Events
- functions passed to event handlers must be passed, not called
- passing a function: tell React to remember it and only call your function when the user clicks the button
- calling a function: fires the function immediately during rendering without any clicks 
	- it fires every time the component renders
- [[arrow functions]]
```js
//passing a function
<button onClick={handleClick}>
<button onClick={() => alert('...')}>


//calling a function
<button onClick={handleClick()}>
<button onClick={alert('...')}>
```

Reading Props in Event handlers
- event handlers are declared inside of a component, they have access to the component's props 

```js
function AlertButton({ message, children }){
	return (
		<button onClick={() => alert(message)}>
			{children}
		</button>
	);
}

export default function Toolbar() {
	return(
		<div>
			<AlertButton message="Playing!">
				Play Movie
			</AlertButton>
			<AlertButton message="Uploading!">
				upload Image
			</AlertButton>
		</div>
	);
}
```
- can pass event handlers as props 
```js
function Button( {onClick, children }){
	return(
		<button onClick={onClick}>
			{children}
		</button>
	);
}
```

Naming Event Handler Props
- can name event handler props any way you like 
- by convention - event handler props should start with on followed by a capital letter

```js
function Button({ onSmash, children }){
	return(
		<button onClick={onSmash}>
			{children}
		</button>
	);
}

export default function App() {
	return(
		<div>
			<Button onSmash={() => alert('Playing!')}>
			Play Movie
			</Button>
			<Button onSmash={() => alert('Uploading!')}>
			Play Movie
			</Button>
		</div>
	)
}

```

- use appropriate HTML tags for your event handlers
	- ex) div > button 
	- using a real browser button enables built-in browser behaviors like keyboard navigation 

```js
export default function Toolbar(){
	return (
		<div className="Toolbar" onClick={() => {
			alert('You clicked on the toolbar!');
		}}>
		<button onClick={() => alert('Playing!')}>
		Play Movie
		</button>
		<button onClick={() => alert('Uploading!')}>
		Upload Image
		</button>
		</div>
	);
}

// stop propagation
// prevent an event from reaching parent components
function Button( {onClick, children}) {
	return(
		<button onClick={e => {
			e.stopProgation();
			onClick();
			}}>
			{children}
			</button>
	);

// passing handlers as alternative to propagation
// can clearly follow the whole chain of code that executes as a result of some event 

function Button({ onClick, children }) {
	return (
		<button onClick={e => {
			e.stopPropatation();
			onClick();
		}}>
			{children}
		</button>
	);
}

// prevent default behavior 
export default function Signup() {
	return(
		<form onSubmit={e => {
			e.preventDefault();
			alert('Submitting!');
			}}>
			<input />
			<button>send</button>
		</form>
	)

}
}
```
Event Propagation
- event propagate upwards 
- event handlers will also catch events from any children your component might have
- event "bubbles" or "propagates" up the tree : starts with where the event happened, and then goes up the tree
- button's onClick will run first, followed by parent's onClick
- all events propagate in React except onScroll, which only works on the JSX tag you attach it to
- event handlers receive an event object as their only argument - can use this object to read information about the event (convention: called e, stands for event )
	- react calls the onClick handler passed to <button>
		- that handler defined in Button
			- calls e.stopPropagation(), preventing the event from bubbling further
			- calls onClick function, which is a prop passed from the Toolbar component
		- function defined in Toolbar component, displays button's own alert
		- since propagation was stopped, the parent <div>'s onClick handler does not run 

Can Event Handlers Have Side Effects?
- event handlers' don't need to be pure 
- it's a great place to change something 



State: A Component's Memory
- Components need to "remember" things: the current input value, the current image, the shopping cart, this kind of component specific memory is called state

```js
  let index = 0;

  function handleClick() {
    index = index + 1;
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleClick}>
        Next
      </button>
```


- why this doesn't work
	- local variables don't persist between renders : when React renders component a second time, it renders it from scratch, doesn't consider any changes to the local variables
	- changes to local variables won't trigger renders, React doesn't realize it needs to render the component again with the new data 
- what it needs
	- retain the data between renders
	- trigger React to render the component with new data(re-rendering)
	- useState hook provides 2 things
		- state variable to retain the data between renders
		- a state setter function to update the variable and trigger React to render the component again 

- What are [[Hooks]]?
	- special functions that are only available while React is rendering 
	- can only be called at the top level of your components on your own Hooks
	- can't call Hooks inside conditions, loops or other nested functions 

- What happens if you use useState
	- your component renders the first time. 0 is initial value, so React remembers 0 is the latest state value
	- you update state by setIndex(index+1), tells React to remember index is 1 now and triggers another render
	- your component's second render, React still sees useState(0) but React remembers you set index to 1, it returns [1, setIndex] instead

- How does React Know Which State to Return?
	- Hooks rely on a stable call order on every render of the same component
	- **this is why only call hooks at the top level is required 
		- this guarantees that hooks will always be called in the same order 
	- React holds an array of state pairs for every component, it maintains **the current pair index which is set to 0 

```js
let componentHooks = [];
let currentHookIndesx = 0;
```


- State is isolated and private
	- state is local to a component instance on the screen, if you render the same component twice, each copy will have completely isolated state, changing one of them will not affect the other 
	- if you want it to share, remove state from child components and add to their closest shared parent 

### Render and Commit

What rendering means in React?
- trigger a render (deliver the guest's order to the kitchen)
- render the component (prepare the order in the kitchen)
- commit to the DOM (place the order on the table)

Step 1: Trigger a render
2 reasons for component to render
- it's component's initial render
- the component's(or one of its ancestors) state has been updated

Step 2: React renders your components
- after trigger a render, React calls your components to figure out what to display on screen
- "rendering" is React calling your component
	- on initial render, React will call the root component
	- for subsequent renders, React will call the function component whose state update triggered the render
- this process is recursive : updated component returns other component -> render that component next -> that component returns something -> render that component next
- during initial render React will create the DOM nodes 
- during re-render, React will calculate which of their properties have changed since the previous render 
- ** if updated component is very high in the tree, default behavior of rendering all components nested within the updated component is not optimal for performance

Step 3: React commits changes to the DOM
- after rendering components, React will modify the DOM
- for initial render, React will use the appendChild() DOM API to put all the DOM nodes it has created on screen
- for re-render, React will apply the minimal necessary operations (calculated while rendering) to make the DOM match the latest rendering output
** React only changes the DOM nodes if there's a difference between renders, 

Browser Paint
- after rendering is done and React updated the DOM, browser will repaint the screen 
- this process is known as "browser rendering", we will refer to it as "painting"