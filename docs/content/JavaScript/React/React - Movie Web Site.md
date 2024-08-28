
What problem is React trying to fix? 
- interactivity simple 

Compare vs JavaScript
- JavaScript: we create html element and then manipulate it 
- react: everything starts first as JavaScript and then it becomes HTML
	- react will update element result in HTML when they need to be updated 
	- can create element, content, registering event listener

- react : like engine that allows us to have interactive UIs
- react-dom: how we put elements in html 

```js
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Document</title>
</head>

<body>
	<div id="root"></div>
</body>
<script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>

<script>

const root = document.getElementById("root");
const span = React.createElement("span", {id: "sexy-span"}, "Hello");
const btn = React.createElement("button", {
onClick: () => {console.log("I'am clicked")}

}, "CLick Me");

const continer = React.createElement("div", null, [span, btn]);
ReactDOM.render(continer, root);

  

</script>
</html>

```


```js
//React Object
1. Children: {map: ƒ, forEach: ƒ, count: ƒ, toArray: ƒ, only: ƒ}
2. Component: ƒ v(a,b,e)
3. Fragment: Symbol(react.fragment)
4. Profiler: Symbol(react.profiler)
5. PureComponent: ƒ K(a,b,e)
6. StrictMode: Symbol(react.strict_mode)
7. Suspense: Symbol(react.suspense)
8. cloneElement: ƒ (a,b,c)
9. createContext: ƒ (a,b)
10. createElement: ƒ ca(a,b,e)
11. createFactory: ƒ (a)
12. createRef: ƒ ()
13. forwardRef: ƒ (a)
14. isValidElement: ƒ M(a)
15. lazy: ƒ (a)
16. memo: ƒ (a,b)
17. useCallback: ƒ (a,b)
18. useContext: ƒ (a,b)
19. useDebugValue: ƒ (a, b)
20. useEffect: ƒ (a,b)
21. useImperativeHandle: ƒ (a,b,c)
22. useLayoutEffect: ƒ (a,b)
23. useMemo: ƒ (a,b)
24. useReducer: ƒ (a,b,c)
25. useRef: ƒ (a)
26. useState: ƒ (a)
27. version: "17.0.2"
28. __SECRET_INTERNALS_DO_NOT_USE_OR_YOU_WILL_BE_FIRED: {ReactCurrentDispatcher: {…}, ReactCurrentOwner: {…}, IsSomeRendererActing: {…}, ReactCurrentBatchConfig: {…}, assign: ƒ, …}
29. [[Prototype]]: Object


//React DOM Object

1. createPortal: ƒ hi(a,b)
2. findDOMNode: ƒ (a)
3. flushSync: ƒ (a,b)
4. hydrate: ƒ (a,b,c)
5. render: ƒ (a,b,c)
6. unmountComponentAtNode: ƒ (a)
7. unstable_batchedUpdates: ƒ ai(a,b)
8. unstable_createPortal: ƒ (a,b)
9. unstable_renderSubtreeIntoContainer: ƒ (a,b,c,d)
10. version: "17.0.2"
11. __SECRET_INTERNALS_DO_NOT_USE_OR_YOU_WILL_BE_FIRED: {Events: Array

```



#### [[JSX]]
- syntax extension to JavaScript
- allow to write HTML-like markup inside JavaScript file

```js
// creating element
// no JSX
const h3 = React.createElement(
	"h3",
	{
		id: 'title',
		onClick: ()=> { console.log("Hello I'm title")}
	},
	Click Button
)

//JSX
const h3 = (
	<h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
		Hello I'm title
	</h3>
);
)
```

JSX Error
- need babel(code transformer so that browser understands)
```shell
index.html:60 Uncaught SyntaxError: Unexpected token '<' (at index.html:60:9)Understand this error
```

```js
<script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">

const root = document.getElementById("root");
const span = React.createElement("span", {id: "sexy-span"}, "Hello");

// const btn = React.createElement("button", {
// onClick: () => {console.log("I'am clicked")}
// }, "CLick Me");

const btn = (
<button
onClick={()=> console.log("I'm Clicked!")}>
</button>

)

const container = React.createElement("div", null, [span, btn]);
ReactDOM.render(continer, root);

</script>
```


How to change createElement to JSX 
-> make element into function 
```js
const Button = () => (
	<button
	onClick={() => console.log("I'm Clicked")}	
></button>
)

function Button2(){
	return(
		<button></button>
	)
}

const container = (<div> <Button/> <Button2/> </div>)

```



Why do we need useState?
- after we made function to update counter, we want that counter to be shown again
- basically we want to run `ReactDOM.render(<Container />,root)` everytime we update counter
- react only updates what needs to be changed, instead of recreating button, h3 elements
```js
....
const root = document.getElementById("root");
let counter = 0;
function countUp() {
	counter = counter + 1;
	ReactDOM.render(<Container />, root);
}
const Container = () => (
	<div>
		<h3>Total clicks: {counter}</h3>
		<button onClick={countUp}>Click me</button>
	</div>
);
ReactDOM.render(<Container />, root);
</script>
</html>



```


When we pass event, React passes SyntheticBaseEvent, and it has nativeEvent(native JavaScript event)

Point of connecting input with value attribute - to manipulate input value outside - like resetting input 

If there is state change in parent component, child component rerenders -> to prevent or optimize you can use memo

Helps to check prop-types
```js
<script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"> </script>

// PropTypes
1. PropTypes: {array: ƒ, bool: ƒ, func: ƒ, number: ƒ, object: ƒ, …}
2. any: ƒ ()
3. array: ƒ ()
4. arrayOf: ƒ createArrayOfTypeChecker(typeChecker)
5. bool: ƒ ()
6. checkPropTypes: ƒ checkPropTypes(typeSpecs, values, location, componentName, getStack)
7. element: ƒ ()
8. elementType: ƒ ()
9. exact: ƒ createStrictShapeTypeChecker(shapeTypes)
10. func: ƒ ()
11. instanceOf: ƒ createInstanceTypeChecker(expectedClass)
12. node: ƒ ()
13. number: ƒ ()
14. object: ƒ ()
15. objectOf: ƒ createObjectOfTypeChecker(typeChecker)
16. oneOf: ƒ createEnumTypeChecker(expectedValues)
17. oneOfType: ƒ createUnionTypeChecker(arrayOfTypeCheckers)
18. resetWarningCache: ƒ ()
19. shape: ƒ createShapeTypeChecker(shapeTypes)
20. string: ƒ ()
21. symbol: ƒ ()
22. [[Prototype]]: Object


Btn.proptypes = {
	
}
```


Why do we need useEffect?
- you want to run certain code in first render like API 
- not get data again when state changes 


Map
- function will be run for every array, and new array with new value will be returned


API link: 
https://yts.mx/api/v2/list_movies.json?minimum_rating=8.5&sort_by=year

#### React Class Component
- they don't have return, but render
- there are functions that are called before render and after render
	- mount
	- update
	- unmount(page navigation)
```js
class App extends React.Component{
	state = {
		count: 0 
	};
	componentDidMount() {
		setTimeout(()=>{
			this.setState({ isLoading: false });
		}, 6000);
	}
	render(){
		return <h1>I'm a class component{this.state.count} </h1>;
	}
}
```