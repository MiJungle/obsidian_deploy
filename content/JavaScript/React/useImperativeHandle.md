- lets you customize the handle exposed as a ref
- parameter:
	- ref
	- createHandle 
- return: undefined


```js
//MyInput.js
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props,ref){
	const inputRef = useRef(null);

	useImperativeHandle(ref, () => {
		return {
			focus() {
				inputRef.current.focus();
			},
			scrollIntoView() {
				inputRef.current.scrollIntoView();
			},
		};
	}, []);
	return <input {...props} ref={inputRef} />;
})

export default MyInput;

//App.js
import { useRef } from 'react';
import MyInput from './MyInput.js';

export default function Form() {
	const ref = useRef(null);

	function handleClick() {
		ref.current.focus();
	}

	return (
		<form>
			<MyInput placholder="Enter your name" ref={ref}/>
			<button type="button" onCLick={handleClick}>
				Edit
			</button>
		</form>
	);
}
```


### Pitfall
- do not overuse refs 
- use refs for imperative behaviors that you can't express as props
	- ex) scrolling to node, focusing a node, triggering animation, selecting text 