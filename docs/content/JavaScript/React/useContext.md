- lets your read context, data from your component
- looks for the closest provider above the component that calls it 
	- doesn't matter how may layers of components there are between the provider and the Button
	- searches upwards and does not consider providers in the component from which you're calling useContext()
	- if cannot find context in the parent tree, value returned by useContext() will be equal to the default value that you specified when you created that context
	- this default value does not change 
- usage
	- accumulate information 

```js
import { createContext, useContext } from 'react';

const ThemeContext = createContext(null);

export default function MyApp(){
	return(
		<ThemeContext.Provider value="dark">
			<Form/>
		</ThemeContext.Provider>
	)

}

function Form(){
	return(
		<Panel Title="Welcome">
			<Button>Sign up</Button>
			<Button>Log in</Button>
		</Panel>
	);
}

function Panel({ title, children }){
	const themme = useContext(ThemeContext);
	const className = 'panel-' + theme;
	return(
		<section className={className}>
			<h1>{title}</h1>
			{children}
		</section>
	)
}

function Button({ children }){
	const theme = useContext(ThemeContext);
	const className = "button-" + theme;
	return(
		<button className={className}>
			{children}
		</button>
	)

}
```

### Updating data : combine it with state
```js
function MyPage(){
	const [theme, setTheme] = useState("dark");
	return(
		<ThemeContext.Provider value={theme}>
			<Form />
			<Button onClick={() =>{
				setTheme("light");
			}}>
			Switch to light theme
			</Button>
		</ThemeContext.Provider>
	)

}
```


### Scaling up with context and a reducer
- create context, create reducer
- pass dispatch through context 
- import context to the top level component - App.js
```js
import { createContext, useContext, useReducer } from 'react';

const TasksContext = createContext(null);
const TasksDispatchContext = createContext(null);

export function TasksProvider({ children }){
	const [tasks, dispatch] = useReducer(
		tasksReducer,
		initialTasks
	);

	return(
		<TasksContext.Provider value={tasks}>
			<TasksDispatchContext.Provider = valeu={dispatch}>
				{children}
			</TasksDispatchContext.Provider>
		</TasksContext.Provider>
	)

}

export function useTasks(){
	return useContext(TasksContext);
}

export function useTasksDispatch(){
	return useContext(TasksDispatchContext);
}

function taskReducer(tasks, action){
	switch(action.type) {
		case 'added': {
			return [...tasks, {
				id: action.id,
				text: action.text,
				done: false
			}]
		}
		case 'changed': {
			return tasks.map(t => {
				if(t.id === action.task.id){
					return action.task;
				} else {
					return t;
				}
			})
		}
		case 'deleted': {
			return tasks.filter(t => t.id !== action.id);
		}
		default: {
			throw Error("Unknown action: " + action.type);
		}
	}
}

const initialTasks = [
  { id: 0, text: 'Philosopher’s Path', done: true },
  { id: 1, text: 'Visit the temple', done: false },
  { id: 2, text: 'Drink matcha', done: false }
];

```


### Overriding context
- you can override context by wrapping that part in a provider with a different value
```js
<ThemeContext.Provider value="dark">
// receives light 
	<ThemeContext.Provider value="light">
		<Footer />
	</ThemeContext.Provider>
	...
</ThemeContext.Provider>
```