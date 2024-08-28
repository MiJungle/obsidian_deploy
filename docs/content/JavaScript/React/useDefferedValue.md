- lets you defer updating apart of the UI
- parameter
	- value: value you want to defer
	- initialValue(optional): value to use during the initial render of a component 
- returns 
	- initial render, returns same value as you provided 
	- during updates, attempt a re-render with old value and then try another re-render in the background with the new value
ex) 
- during initial render: deferred value will be the same as the value you provided
- during updates: the deferred value will "lag behind" the latest value 
	- React will first re-render without updating the deferred value, then try to re-render with the newly received value in the background

```js
const deferredValue = useDeferredValue(value, initialValue?);

//ex) 
import { useSate, useDeferredValue } from 'react';

from SearchPage(){
	const [query, setQuery] = useState('');
	const deferredQuery = useDeferredValue(query);
}
```


### Caveats
- useDeferredValue is integrated with `<Suspense>` 
	- if background update caused by a new value suspends the UI, user will not see the fallback, they will see the old deferred value until the data loads 
- deferredQuery will keep its previous value until the data has loaded

```js
// using suspense, show previous results -> loading -> update -> show updated results

import { Suspense, useState } from 'react';
import SearchResults from './SearchResults.js';

export default function App(){
	const [query, setQuery] = useState('');
	return(
		<>
			<label>
			search albums:
				<input value={query} onChange={e => setQuery(e.target.value)}/>
			</label>
			<Suspense fallback={<h2>Loading...</h2>}>
				<SearchResults query={query} />
			</Suspense>
		</>
	)

}


// using deferredQuery, shows previous results until new results are ready 
// show previous results -> update -> show updated results
export default function App(){
	const [query, setQuery] = useState('');
	const deferredQuery = useDeferredValue(query);

	reuturn(
		<>
			<label>
				Search albums:
				<input value={query} onChange={e => setQuery(e.target.value)} />
			</label>
			<Suspense fallback={<h2>Loading...</h2>}>
				<SearchResults query={deferredQuery} />
			</Suspense>
		</>
	)

}
```


2 steps
- React re-renders with the new query ("ab") but with the old deferredQuery (still "a"). The deferredQuery which you pass to the result is deferred : lags behind the query value
- in the background, React tries to re-render with both query and deferredQuery updated to "ab". If this re-render completes, React will show it on the screen 
	- if suspends, React will abandon this rendering attempt, and retry this re-render again after the data has loaded
	- meaning, old value will be seen until data is ready 
- deferred background rendering is interruptible 
	- if you type into input again, React will restart with the new value, React will always use the latest provided value 

What if user does not know it is updating ? -> give CSS transition , slightly dimming the words 
```js

import { Suspense, useState, useDeferredValue } from 'react';
import SearchResults from './SearchResults.js';

export default function App(){
	const [query, setQuery] = useState('');
	const deferredQuery = useDeferredValue(query);
	const isStale = query !== deferredQuery;

	return(
		<>
			<label>
				Search albums:
				<input value={query} onChange={e => setQuery(e.target.value)}/>
			</label>
			<Suspense fallback={<h2>Loading...</h2>}>
				<div style={{
					opacity: isStale? 0.5 : 1,
					transition: isStale ? 'opacity 0.2s 0.2s linear' : 'opacity 0s 0s linear'
				}}>
			<SearchREsults query={deferredQuery} />
		</div>
		</Suspense>
	)

}
```


### [[Performance Optimization]]
- useful when a part of your UI is slow to re-render


### Deferring Value vs [[Debouncing]] vs[[Throttling]]
Debouncing: wait for the user to stop typing before updating the list 
Throttling: update the list every once in a while 
Deferring: 
- deeply integrated with React itself 
- doesn't require choosing any fixed delay 
	- can make it depend on user device & environment 
- interupted by default, if React is in the middle of the re-rendering a large list, user makes another keystroke, React will abandon that re-render, handle the keystroke and start rendering in the background 
	- debouncing and throttling - blocking, merely postpone the moment 
