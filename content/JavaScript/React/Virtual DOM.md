Lightweight copy of actual DOM 
- it's exactly same as actual DOM but doesn't have power to directly change the layout of the document 
- manipulating virtual DOM is fast as nothing gets drawn on the screen 

- Virtual DOM is a programming concept where a virtual representation of a UI is kept in memory synced with “Real DOM ” by a library such as ReactDOM and this process is called reconciliation

Virtual representation of UI is kept in memory and synced with the "real" DOM by a library such as ReactDOM. 
- enables declarative API of React
	- tell React what state you want the UI to be in 
- abstracts out attribute manipulation, event handling, 


Virtual DOM works in 3 simple steps
[[Reconciliation]]
- whenever any underlying data changes, the entire UI is re-rendered in Virtual DOM representation 
- The Difference between the previous DOM representation and the new one is calculated
	- comparing current Virtual DOM with previous Virtual DOM is [[diffing]]
- Once the calculations are done, real DOM will be updated with only the things that have actually changed
	- finds best possible ways to make changes to the real DOM 
	- uses batch updates, meaning changes to the real DOM are sent in batches instead of sending any update for a single change in the state of a component
		- grouping multiple changes together into a single update operation 
		- React collects for example state changes and then perform updates in a single operation 



Initial Render
- When a React component first renders, React constructs a VDOM tree representing the UI structure in memory. This VDOM is a lightweight copy of the actual DOM 

Reconciliation
- when the state or props of a component change, React creates a new VDOM tree that reflects these changes. This process is called reconciliation

Diffing Algorithm
- React compares the new VDOM tree with the previous VDOM tree using a diffing algorithm. 
	- diffing algorithm: identifies the minimal number of changes required to update the real DOM to match the new VDOM. The comparison happens in three main steps 


Real DOM
```js
document.getElementById('some-id').innerValue = 'updated value';
```
- what happens: 
	- browser parses HTML to find the node with this id
	- removes the child element of this specific element
	- updates element(DOM) with the 'updated value'
	- recalculates the CSS for the parent and child nodes 
	- update the layout
	- traverse tree and paint it on the screen(browser) display
- recalculating the CSS and changing the layouts involves complex algorithms, affect performance
- => react created virtual DOM 

[[Shadow DOM]]
- browser technology designed primarily for scoping variables and CSS in web components 
- Virtual DOM is concept implemented by libraries in JavaScript on top of browser APIs


https://www.geeksforgeeks.org/reactjs-virtual-dom/ 
https://www.geeksforgeeks.org/courses/full-stack-node?utm_campaign=241_abt&utm_medium=abt&utm_source=gfgcontent