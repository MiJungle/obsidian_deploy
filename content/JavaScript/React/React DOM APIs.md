- supported for the web applications, not for React Native

#### APIs
- createPortal: lets you render child components in a different part of the DOM tree
- flushSync: lets you force React to flush a state update and update the DOM synchronously 

#### Resource Preloading APIs
- used to make apps faster by pre-loading resources such as scripts stylesheets, fonts 
- React-based frameworks
	- prefetchDNS : lets you prefetch the IP address of a DNS domain name that you expect to connect to 
	- preconnect: lets you connect to server you expect to request resources from 
	- preload: lets you fetch a stylesheet, font, image, external script that you expect to use 
	- preloadModule: lets you fetch an ESM module that you expect to use 
	- preinit: lets you fetch and evaluate an external script or fetch and insert a stylesheet
	- preinitModule: lets you fetch and evaluate an ESM module 

#### Entry Points
- react-dom/client: contains APIs to render React components on the client
- react-dom/server: contains APIs to render React components on the server


#### Deprecated APIs
- findDOMNode : finds closest DOM node corresponding to a class component instance
- hydrate: mounts a tree into the DOM created from server HTML, use hydrateRoot
- [[render]]: mounts a tree into the DOM, deprecated in favor of createRoot
- unmountComponentAtNode: unmounts a tree from the DOM, use root.unmount()

