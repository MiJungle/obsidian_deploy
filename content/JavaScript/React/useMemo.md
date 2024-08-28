-  lets you cache the result of a calculation between re-renders until its dependencies change
- use this for only performance optimization 
```js
//
useMemo(calculateValue, dependencies)
//

import { useMemo } from 'react';

function TodoList({ todos, tab }){
	const visibleTodos = useMemo(
		() => filterToodos(todos, tab),
 	), [todos, tab]
}
```

#### Parameters
- calculateValue: function calculating the value that you want to cache 
	- should be pure
	- take no arguments
	- return a value of any type 
	- React will call function during the initial render 
	- next render - React will return the same value again if the dependencies have not changed since the last render, otherwise it will call calculateValue, return its result and store it so it can be reused it later
- dependencies
	- list of all reactive values referenced inside the calculateValue code 

#### Returns
- initial render: returns the result of calling calculateValue with no arguments
- next render: either return an already stored value form the last render (if dependencies did not change) || calculateValue again, and return the result that calculateValue has returned

#### Caveats
- strict mode : React will call your calculation function twice in order to help you find accidental impurities 
	- development-only behavior , X affect production 
- React will not throw away the cached value unless there is specific reason to do that 
	- ex) development : React throws the cache when you edit the file of your component 
	- React will throw away the cache if your component suspends during the initial mount

#### Suspension
- suspends refers to a component pausing its rendering process because it is waiting for some asynchronous data to be fetched or some other async operation to complete 
- ex) if component is fetching data from an API and that fetch operation has not completed yet, the component will suspend until the data iks available 



#### Usage
- skipping expensive recalculations
- ex) if you are filtering or transforming a large array, or doing some expensive computation, you might want to skip doing it again if data isn't changed

- optimizing with useMemo is only valuable in few cases:
	- calculation is slow, dependencies rarely change
	- skip re-rendering if value hasn't changed

- memoizing individual JSX nodes
```js
export default funciton TodoList({ todos, tab, theme }){
	const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
	const children = useMemo(() => <List items={visibleTodos} />, [visibleTodos]);
	return(
		<div className={theme}>
			{children}
		</div>
	)
}
```





#### How to tell if a calculation is expensive?
- add console log to measure the time 
	- if overall logged time adds up to significant amount like 1ms or more, memoize calculation
- note that useMemo **won't make the first render faster**. It only helps you skip unnecessary work on updates
```js
console.time('filter array');
const visibleTodos = filterTodos(todos, tab);
console.timeEnd('filter array');
```