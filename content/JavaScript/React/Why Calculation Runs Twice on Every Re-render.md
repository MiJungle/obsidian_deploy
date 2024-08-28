- Strict Mode: React will call some of your functions twice instead of once
	- React uses the result of one of the calls, ignores result of the other call 
	- if component & calculation functions are pure, this shouldn't affect logic 

#### Why?
- helps keep components pure
	- avoid bugs & unpredictable behavior
	- what is pure
		- minds own business: doe not change any objects or variables that existed before it was called
		- same inputs, same output: given the same inputs, a pure function should always return the same result

#### Example
- todo will be added twice
```js
const visibleTodos = useMemo(() => {
// mutates todos 
	todos.push({ id: 'last', text: 'Go for a walk!'});
	const filtered = filterTodos(todos, tab);
	return filtered;
}, [todos, tab]);
```
- pure
```js
const visibleTodos = useMemo(() => {
	const filtered = filterTodos(todos, tab);
	filtered.push({ id : 'last', text: 'Go for a walk!' });
	return filtered;
}, [todos, tab]);
```
```