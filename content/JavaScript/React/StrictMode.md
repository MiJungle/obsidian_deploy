- lets you find common bugs in your components early during development
- enable additional development behaviors and warnings

```js
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(
	<StrictMode>
		<App />
	</StrictMode>

)


```



Enables development-only Behaviors
- components will re-render an extra time to find bugs caused by impure rendering
- components will re-run Effects an extra time to find bugs caused by missing Effect cleanup
- components will be checked for usage of deprecated APIs
(no way to opt out Strict Mode inside a tree rapped in `<StrictMode>`)


React assumes every component you write is a pure function
- to help you find accidentally impure code, Strict Mode calls some of your functions twice in development
	- component function body 
	- functions that you pass to useState, set functions, useMemo, useReducer
	- constructor, render shouldComponentUpdate
- if it is pure, running it twice does not change its behavior as pure function produces same result every time 

```js
export default function StoryTray({ stories }){
	const items = stories.slice(); // Clone the array
	items.push({ id: 'create', label: 'Create Story' });
}
```

```js
useEffect(() => {
	const connection = createConnection(serverUrl, roomId);
	connection.connect();
	// cleans up, otherwise active conneciton num will increase
	return () => connection.disconnect(); 
	}, [roomId]);

})
```