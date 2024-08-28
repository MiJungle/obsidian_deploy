- lets you update the state without blocking the UI

```js
startTransition(scope)
```

```js
import { startTransition } from 'react';

function TabContainer() {
	const [tab, setTab] = useState('about');

	function selectTab(nextTab) {
		startTransition(() => {
			setTab(nextTab);
		})
	}
}

```


#### Parameters
- scope: function that updates some state by calling one or more set functions 
	- React immediately calls scope with no arguments and marks al state updates scheduled synchronously during the scope function call as Transitions 
	- will be non-blocking & will not display unwanted loading indicators

#### Returns
- no return 

#### Caveats
- starTransition does not provide a way to track whether a Transition is pending 
	- for pending indicator, need useTransition instead
- function you pass must be synchronous 
	- if you try to perform more state updates later, they won't be marked as Transitions 
- if there are multiple ongoing Transitions, React batches them together


#### Usage
- can let some updates without waiting for first render to finish 
- ex) if user clicks a tab but then change their mind and click another tab, they can do that without waiting for the first re-render to finish 

