Scope
- var : function level scope
	- variable declared in block level scope is also accessible in global level 
```js
// Block Level
{ var x =2; } console.log(x) //2
{ let x =2; } console.log(x) //x is not defined

// Function Level
function foo() {
	var a = 5;
}
console.log(a) // a is not defined

```

Dynamic Scope vs Static Scope 

