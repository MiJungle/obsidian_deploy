- allows inserting elements into the DOM before any layout Effects fire 
- allows insert styles before any Effects fire that may need to read layout
- unless working on a CSS-in-JS library authors || need place to inject the styles, use useEffect, useLayoutEffect 
- parameters
	- setup : function 
		- React will run cleanup -> setup -> cleanup 
	- dependency(optional)
	- return : undefined

#### Caveats
- run on the client, don't run during server rendering 
- can't update state from inside useInsertionEffect
- when useInsertionEffect runs, refs are not attached 


#### What is CSS-in-JS 
- static extraction to CSS files with a compiler
- inline styles e.g. `<div style={{ opacity: 1}]>`
- runtime injection of `<style>` tags
***recommended a combination of the first 2 approaches
don't recommend runtime injection 
 - runtime injection forces the browser to recalculate the styles a lot more often
 - runtime injection can be very slow if it happens at the wrong time in the React lifecycle
-> useInsertionEffect can be better than inserting styles during useLayoutEffect or useEffect because it ensures that by the time other Effects run in your components, `<style` tags have already been inserted 



```js
let isInserted = new Set();
function useCSS(rule) {
	useInsertionEffect(() => {
		if(!isInserted.has(rule)){
			isInserted.add(rule);
			document.head.appendChild(getStyleForRule(rule));
		}
	});
	return rule;
}

function Button() {
	const className = useCSS('...');
	return <div className={className} />;
}

```