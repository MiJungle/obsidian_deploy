- APIs that let you render React components to HTML on the server 
- only used on the server at the top level of your app to generate the initial HTML

#### Server APIs for Node.js Streams
- [[renderToPipeableStream]] : renders a React tree to a pipeable Node.js Stream
- [[renderToStaticNodeStream]] : renders a non-interactive React tree to a Node.js Readable Stream


#### Server APIs for Web Streams
- these methods are only available in the environments with Web Streams, which includes browsers Deno and some modern edge runtimes
- [[renderToReadableStream]] renders a React tree to a Readable Web Stream


#### Server APIs for non-streaming environments
- methods can be used in the environments that don't support streams:
- [[renderToString]] : renders a React tree to a string
- [[renderToStaticMarkup]] : renders a non-interactive React tree to string
