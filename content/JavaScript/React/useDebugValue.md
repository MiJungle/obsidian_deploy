- lets you add a label  to a custom Hook in React Dev Tools
- does not modify or return data to be used by the component logic or lifecycle but only adds debug information for tools 
	- parameters
		- value: value you want to display in React DevTools
		- format(optional): formatting function, React DevTools will call the formatting function with the value as the argument, and display the returned formatted value
	- returns 
		- does not return anything 
```js
useDebugValue(value, format?)

// ex)
import { useDebugValue } from 'react';

funciton useOnlineStatus(){
	useDebugValue(isOnline ? 'Online' : 'Offline');
}
```


- Don't add debug values to every custom Hook. 
- Use it where there is complex internal data structure that's difficult to inspect 
- 