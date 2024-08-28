- can hurt performance, prefer useEffect when possible 
	- code inside and all state updates scheduled from it block the browser from repainting the screen
- version of useEffect that fires before the browser repaints the screen
- usage: perform layout measurements before the browser repaints the screen
	- ex) tooltip that appears next to some element on hover 

1) Render the initial content
2) Measure the layout before the browsing repaints the screen
3) Render the final content using the layout information you've read


```js
import { useState, useRef, useLayoutEffect } form 'react';

function Tooltip(){
	const ref = useRef(null);
	const [tooltipHeight, settooltipHeight] = useState0);

	useLayoutEffect(() => {
		const { height } = ref.current.getBoundingClientRect();
		setTooltipHeight(height);
	}, []);

}
```


#### Parameters
- setup: function with Effect's logic 
	- function may return cleanup function 
	- before component is added to DOM, React will run setup funciton 
	- after every re-render with changed dependencies, React will first run clean up -> setup with new values 
	- before your component is removed from the DOM, React will run your cleanup function
- dependencies(optional)


#### Caveats
- Strict Mode: React will run 1 extra development-only setup+cleanup cycle before the first real setup 
	- to ensure your cleanup logic "mirrors" your setup logic and that it stops or undoes whatever the setup is doing