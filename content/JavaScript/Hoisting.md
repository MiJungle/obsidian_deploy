Hoisting in [[JavaScript]] is a term used to describe how the JavaScript engine processes variable and function declarations during the compilation phase, before the execution of the code. 

Despite the common metaphor that declarations are "moved" to the top, hoisting actually involves how the engine registers identifiers in memory. 

When [[JavaScript Engine]] compiles your code, it registers variables and funcitons in memory before executing the code. This means that these identifiers (name of variables and functions) are known to the engine throughout their entire scope even if they are declared later in the actual code. 

### 1. Variable Hoisting
Variable declarations using 'var' are hoisted to the top of their function scope or global scope. Only declarations are hoisted, not the initializations
```js
console.log(message); // undefined
var message = 'Hello, world';
console.log(message); // Hello, world
```


### 2. Function Hoisting
Function declarations are hoisted to the top of their scope, allowing them to be called before they appear in the code. 

```js
greet(); // Hello, world!
function greet(){
	console.log('Hello World!');
}
```


### 3. 'let' and 'const' Hoisting
Variables declared with 'let' and 'const' are also hoisted but not initialized. They are place din a "[[Temporal Dead Zone]]" (TDZ) from the start of the block until the declaration is encountered, leading to 'ReferenceError' if accessed before initialization. 
```js
console.log(greeting); // ReferenceError: Cannot access 'greeting' before initialization
let greeting = 'Hello!';
console.log(greeting); // Hello!
```


### 4. Function Expressions and Arrow Functions
- sum is treated as a variable, 'var sum' is hoisted, but the function assignment happens where it appears in the code, 'sum' is 'undefined' when called before the assignment, leading to a type error
```js
console.log(sum(5, 10)); // TypeError: sum is not a function
var sum = function(a,b) {
	return a + b;
}

console.log(sum(5, 10)); // ReferenceError: sum is not defined
let sum = function(a,b) {
	return a + b;
}

console.log(sum(5, 10)); // ReferenceError: sum is not defined
let sum = () => {
    return a + b;
}
```


### Review
- shadowing : The inner variable **hides** or **shadows** the outer variable within its scope
	- Shadowing does not remove or replace the outer variable; it merely makes the outer variable inaccessible within the inner scope.
	- The outer variable is still accessible outside the inner scope.
```js
var a = 1; 
function outer() { 
	console.log(a);
	var a = 2;
} // Output: undefined var a = 2; } outer();
outer();
```



자바스크립트 엔진이 코드를 평가하면서 식별자를 미리 환경 레코드 (Environmental Record)에 등록하면서 자연스럽게 생기는 현상. 

**쉽게 설명하기 위해 마치 함수와 변수 선언이 코드의 최상단에 끌어올려져 있는 듯한 현상이라고 하지만, 진짜로 변수 선언문이 위로 끌어올리는 것은 아니다. 

자바스크립트 엔진은 코드 평가 단계에서 실행 컨테스트를 생성하고, 코드 내의 선언문들을 파악하여 실행 컨텍스트 내의 환경 래코드에  식별자들을 등록시킨다. 따라서 선언문보다 변수 사용이 앞에 있더라도 에러가 발생하지 않음. 
- var
- function


