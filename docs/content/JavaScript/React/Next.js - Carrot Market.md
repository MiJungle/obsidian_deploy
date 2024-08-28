#### What We Are Using
- Prisma: Object Relation Mapper
	- write / describe shape of data base => Prisma transform format to SQL queries, database
	- generate client to talk to database directly 
- PlanetScale: serverless database, easy to scale
- vercel 
- Cloudflare:
	- 1) image : handle file upload (cheap & optimized for image)
	- 2) stream : nice for uploading video, live stream
- Tailwind CSS: consider it as super huge css file with classNames 
	- puts design in content



#### Tailwind CSS
implemented for mobile screen 
- modifiers : conditions like hover, selected etc
```js
<span className="hover:bg-green-500 hover:scale-150"></span>
```
- responsive modifiers :
	- sm: minimum, means bg-red from small to bigger screen 
		- so from small to medium red, from medium ~ green
```js
<span className="sm:bg-red-100 md:bg-green-100"> 
```
- form modifiers
```js
// gradient, invalid sudo class 
<button className="bg-gradient-to-tr via-yellow-400 from-cyan-500 to-purple-400 invalid:focus:bg-red-100">

// peer: sibling
<input className="peer">
<button className="peer:invalid:bg-red-100">
```
- state modifiers
	- `*:` will be applied to all the children
	- `has-[]` : specific children that meets condition ex) peer, invalid .. 
```js
<div className="*:outline-none *:bg-gray-800 has[peer]:bg-red-100"> <div> </div> </div>
```
- group modifiers
	- change children depending on parent's state
	- define group 
```js
<div className="group">
	<div className="group-hover:bg-red-100">

```
- theme : dark, light
```js
<span className="dark:bg-gray-700 dark:text-gray-300"></span>
```
- ring : tailwind border
```js
<span className="focus:ring-orange-500 focus:ring-offset-2

.ring {  
--tw-ring-offset-shadow: var(--tw-ring-inset) 0 0 0 var(--tw-ring-offset-width) var(--tw-ring-offset-color);  
--tw-ring-shadow: var(--tw-ring-inset) 0 0 0 calc(3px + var(--tw-ring-offset-width)) var(--tw-ring-color);  
box-shadow: var(--tw-ring-offset-shadow), var(--tw-ring-shadow), var(--tw-shadow, 0 0 #0000);
}
```

- list and animations
	- odd, even, last, first
	- animations: animate-bounce, animate-spin, animate-pulse
	- empty: styles for empty elements
```js
<div className="odd:pd-5 last:pb-0 empty:h-5"> 
	<div></div>
	<div></div>
</div>
```

- Just Intime Compiler 
- Tailwind is compiler 
	- compiler read className will generate css for you
	- arbitrary value `h-[138.4px]` `bg-[#54342]`
```js
//tailwind.config.ts
// tells compiler to look into classNames in pages ~~
content: [

"./pages/**/*.{js,ts,jsx,tsx,mdx}",
"./components/**/*.{js,ts,jsx,tsx,mdx}",
"./app/**/*.{js,ts,jsx,tsx,mdx}",

],
theme: [
	extend: {
		border-radius: {
			"something-name": "11.11px"
		}
	}
],
plugin: [] // adds style to base, components, utilities styles 
// daisy ui

//can create repeated styles and use it 
<div className="someting-name ,,,, ">
```

- Directive
	- base: base styles(kind of reset browser default styles)
	- utilities: compiler put all className
	- apply: allows you to re-use styles
	- components: encapsulates classes 
```js
@tailwind base;
@tailwind components;
@tailwind utilities;

//general
@layer base {
	a {
		@apply text-blue-500;
	}
}

//new class you can use
@layer utilities {
	.text-bigger-hello {
		@apply text-3xl
	}
}

// abstracts for re-use
@layer components {
	.btn {
	"bg-gray-300 h-screen flex items-center justify-center p-5
	}
}

.btn {
	@apply w-full bg-black ...
}
```