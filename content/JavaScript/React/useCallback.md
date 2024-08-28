Lets you cache a function definition between re-renders
```js
const cachedFn = useCallback(fn, dependencies)
```


```js
import { useCallback } from 'react';

export default function ProductPage({ productId, referrer, theme }){
	const handleSubmit = useCallback((orderDetails) => {
		post('/product/' + productId. + '/buy', {
			referrer,
			orderDetails,
		});
	}, [productId, referrer]);

}
```

### Parameters
- fn: function you want to cache 
	- React will return(not call) your function back to you during initial render 
	- next render, React will give you the same function again if the dependencies have not changed since the last render 
	- function is returned so you can decide when and whether to call it 
- dependencies: list of reactive values referenced inside of the fn code 
	- React will compare each dependency with its previous value using Object.is 


### Useful Cases
- to optimize rendering performance, sometimes need to cache the functions that you pass to child components 
- why optimize?
	- by default when a component re-renders, React re-renders all of its children recursively 
	- in the example below, ShippingForm props will never be the same
		- in Javascript a function() {} or () => {} always creates different function 
		- useCallback does not prevent creating the function, you are always creating a function, but React ignores it and gives you back a cached function if nothing changed
```js
function createFunction() { return function() {}; } 
const func1 = createFunction(); 
const func2 = createFunction(); 
console.log(func1 === func2); // Output: false

// every time ParentComponent renders, handleClick is a new function 
function ParentComponent() { 
	const handleClick = () => console.log('Clicked'); 
	return <ChildComponent onClick={handleClick} />; }

```


```js
function ProductPage({ productId, referrer, thme }){
	const requirements = useMemo(() => {
		return computerRequirements(product);
	}, [product]);

	function handleSubmit(orderDetails){
		post('/product/' + productId + '/buy/', {
			referrer,
			orderDetails,
		});
	}
	return(
		<div className={theme}>
			<ShippingForm onSubmit={handlesubmit} requirements={requirements}/>
		</div>
	)

}
```



### Difference with [[useMemo]]
- useMemo: caches the result of calling your function 
	- in the example, it caches result of calling computeRequirements(product), doesn't change until product has changed 
	- pass requirements without re-rendering ShippingForm
- useCallback: caches the function itself 
	- does not call the function you provide, caches the function you provided
	- pass handleSubmit without rerendering ShippingForm 