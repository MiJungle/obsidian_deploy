Our project was adding padding-top dynamically through IIFE function. 
Why? to make things easier.

For all pages, there was header and below that there were contents. Since height of header differed among pages, we thought it was more convenient to calculate outer height and add the height dynamically. 

```js

(()=> {

	const headerHeight = $(".header").outerHeight();
	const content = $(".content");
	content.css("padding-top", headerHeight);

})();

```

- pros : can calculate header outer height without having to declare height for every heading 
- cons: too dependent on header element, if for any reason, header element is not drawn in the DOM, height could be null 


Now What?
- situation: add margin below the bottom of the page 
- should it be static or dynamic? 


## 1)  Changing Height to [[CSS]] Style
100% -> 115%
```
HTML, body {
	height: 100%
}
```

Setting height of both HTML and body to 100% is a common approach to ensure that the body takes up the full height of the viewport. It is done to make sure that elements within the body, such as a container or content can be styled to take up the full height . 
-> more about controlling the layout of page and ensuring content stretches to the bottom of the viewport 


## 2) Dynamically Adding Padding 
```
$("body").css("padding-bottom", 0);
```
Adding padding dynamically using JavaScript is more about adjusting the spacing within the body element. It allows you to add extra space at the bottom of the body element, potentially pushing the content away from the edges of the viewport.

++ adding margin-bottom did not work as body's height was set to 100%
When you set the height of the body to 100%, it means the body will take up the full height of its containing element, usually the viewport. So adding 'margin-bottom' will not directly create space at the bottom of the viewport. 

Margin-bottom creates space outside the bottom border of the body element but since the body is already takin g up the full height of the viewport, it won't push elements from the bottom of the viewport.

Padding-bottom will add space inside the body element at the bottom, pushing its content away from the bottom edge(not literally pushing, "expanding")

