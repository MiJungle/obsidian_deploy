- supported for web applications(which run in the browser DOM environment)
- not supported in no-browser environments like iOS, AOS, Windows applications 


#### useFormStatus 
- available in experimental channels
- gives you status information of the last form submission

```js
import { useFormStatus } from "react-dom";
import action from './actions';

function Submit(){
	const status = useFormStatus();
	return <button disabled={status.pending}>Submit</button>
}

export default function App(){
	return(
		<form action={action}>
			<Submit/>
		</form>
	)
}

```