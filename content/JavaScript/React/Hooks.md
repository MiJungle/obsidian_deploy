Features provided by React.

[[State Hooks]]
- lets you remember information like user input 
- [[useState]] declares state variable that you can update directly
- [[useReducer]] declares state variable with the update logic inside a reducer function 
- [[useActionState]] experimental phase, previously part of React DOM called useFormState 



[[Context Hooks]]
- lets component receive information from distant parents without passing it as props 
- [[useContext]]

[[Ref Hooks]]
- lets component hold some information that isn't used for rendering 
- updating ref does not re-render component 
- escape hatch from React paradigm 
- [[useRef]] mostly used to hold a DOM node 
- [[useImperativeHandle]] rarely used, lets you customize the ref 

[[Effect Hooks]]
- lets component connect to and synchronize with external systems 
- dealing with network, browser DOM, animations, UI library 
- escape hatch from React paradigm 
	- don't use it to control data flow 
	- if not interacting with an external system, get rid of effect 
- [[useLayoutEffect]] fires before the browser repaints
- [[useInsertionEffect]] fires before React makes changes to the DOM 

[[Performance Hooks]]
- skip unnecessary work 
- [[useMemo]] lets you cache the result of an expensive calculation
- [[useCallback]] lets you cache a function definition before passing it down to an optimized component 
- if you can't skip re-rendering because screen needs update
	- separate block updates that must be synchronous from non-blocking updates that don't need to block the user interface 
	- [[useTransition]] lets you mark state transition as non-blocking and allow other updates to interrupt it
	- [[useDeferredValue]] lets you defer updating a non-critical part of the UI and let other parts update first 

[[Other Hooks]]
- [[useDebugValue]] lets you customize the label React DevTools displays for your custom Hook
- [[useId]] lets component associate a unique ID with itself
- [[useSyncExternalStore]] lets a component subscribe to an external stoer
- [[useActionState]] allows you to manage state of actions 

[[Custom Hooks]]
