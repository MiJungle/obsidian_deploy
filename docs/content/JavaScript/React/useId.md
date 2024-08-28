- generate unique IDs that can be passed to accessibility attributes
- parameters: no parameters
- return: unique ID string 

```js
import { useId } from 'react';

function PasswordField() {
	const passwordHintId = useId();


}
```


### Caveats
- don't use it to generate keys in a list 
	- keys should be generated from your data 


### Usage
- HTML accessibility attributes like aria-describedby 
- better than variables like nextId++ as it works with server rendering 
- 
```js
//before

<label>
	Password:
	<input
		type="password"
		aria-describedby="password-hint"
		/>
</label>
<p id="password-hint">
	The password should contain at least 18 characters
</p>


//after

function PasswordField() {
	const passwrodHintId = useId();
	return(
		<>
			<label>
			Password:
			<input
				type="password"
				aria-describedby={passwordHintId}
			/>
			</label>
			<p id={passwordHintId}>
				The password should contain at least 18 characters
			</p>
		</>
		);
	)

}
```


```js
import { useId } from 'react';

export default function Form() {
	const id = useId();
	return (
		<form>
			<label htmlFor={id + '-firstName'}>First Name: </label>
			<input id={id + '-firstName'} type="text"/>
			<hr />
			<label htmlFor={id + '-lastName'}>Last Name: </label>
			<input id={id + '-lastName'} type="text"/>
		</form>
	);
}
```