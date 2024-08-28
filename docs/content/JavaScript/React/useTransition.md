- lets you update the state without blocking the UI 
- when states are updated, it re-renders, changing UI but with useTransition, you can change state and not block user interaction
```js
function TabContainer() {
	const [isPending, startTransition] = useTransition();
	const [tab, setTab] = useState('about');

	function selectTab(nextTab) {
		startTransition(() => {
			setTab(nextTab);
		})
	}
}

```

#### Parameters & Returns
Parameters : no parameters
Returns
- array with 2 items
	- isPending: flag that tells you whether there is pending transition
	- startTransition function: lets you mark a state update as a transition

startTransition function
- parameters: 
	- scope: function that updates some state by calling one or more set functions 
	- non-blocking & will not display unwanted loading indicators
- return: no returns
