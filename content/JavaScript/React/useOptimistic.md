- experimental 
- lets you optimistically update the UI
	- show a different sate while an async action is underway 
	- optimistic state: used to immediately present the user with the result of performing an action even though action actually takes time to complete


```js
import { useOptimistic } from 'react';

function AppContainer() {
	const [optimisticState, addOptimistic] = useOptimistic(state, 
	//updateFn
	(currentState, optimisticValue) => {
	
		}
	)
}
```

#### Parameters
- state: value to be returned initially and whenever no action is pending
- updateFn(currentState, optimisticValue)
	- function that takes the current state and the optimistic value passed to addOptimistic and returns the resulting optimistic state
		- pure function 
		- takes 2 parameters: currentSate & optimisticValue

#### Returns
- optimisticState : equal to the value returned by updateFn
- addOptimistic : dispatching function to call when you have an optimistic update 
	- takes 1 argument : optimisticValue 


#### Usage
- helps to make apps feel more responsive
	- provides way to optimistically update the user interface before background operation like a network request completes
- ex) hits send button -> useOptimistic Hook allows message to immediately appear in the list with a "Sending ..." label -> this gives impression of speed and responsiveness 

```js

import { useOptimistic, useState, useRef } from "react";
import { deliverMessage } from './actions.js';

function Thread({ message, sendMessage }){
	const formRef = useRef();
	async function formAction(formData) {
		addOptimisticMessage(formData.get("message"));
		formRef.current.reset();
		await sendMessage(formData);
	}
	const [optimisticMessages, addOptimisticMessage] = useOptimistic(
		messages,
		(state, newMessage) => [
			...state,
		{
			text: newMessage,
			sending: true
		}
	]
);

return (
	<>
		{optimisiticMessages.map((message, index) => (
			<div key={index}>
				{message.text}
				{!!message.sending && <small> (sending...)</small>}
			</div>
		))}
		<form action={formAction} ref={formRef}>
			<input type="text" name="message" placeholder="Hello!" />
			<button type="submit">Send</button>
		</form>
	</>
)
}

export default function App() {

	const [messgae, setMessages] = useState([
		{ text: "Hello there!", sending: false, key: 1 }
	])

	async function sendMessage(formData){
		const sendMessage = await deliverMessage(formData.get("message"));
		seMessages((messages) => [...messages, { text: sentMessage }]);
	}
	return <Thread messages={messgaes} sendMessages={sendMessage} />;
}


```