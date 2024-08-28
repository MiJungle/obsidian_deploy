
Consists of 
- environment record : internal data structure that holds variable bindings and function declarations
- outer environment reference : a link to the parent(outer) lexical environment  


```js
{
	environmentalRecord: {
		variable1: value,
		variable2: value2
	},
	outer: <reference to parent environment>
}
```


```js
function outer(){
	let outerVar = "I am outside!";

	function inner(){
		let innerVar = "I am inside!";
		console.log(outerVar);
	}
	inner();
}
outer();

// global environment
{
environmentRecord: {
	outer: <function outer>
},
outer: null // NO parent environment
}

// lexical environemnt of outer 
{
environmentRecord: {
	outerVar: 'I am outside!',
	inner: <function inner>
},
	outer: <global environment>
}

// lexical environemnt of inner
{
environmentRecord: {
	innerVar: "I am inside!"
}
	outer: <lexical environment of outer> 
}

```


