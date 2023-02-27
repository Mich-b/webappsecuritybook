# Cross-origin resource sharing
Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to tell browsers to give a web application running at one origin access to selected resources from a different origin. In other words, CORS provides a means to relax the same origin policy. A web application executes a cross-origin HTTP request when it requests a resource that has a different origin (domain, protocol, or port) from its own.

As we've seen before, browsers restrict cross-origin HTTP requests initiated from scripts for security reasons. For example, XMLHttpRequest and the Fetch API follow the same-origin policy. This means that a web application using those APIs can only request resources from the same origin the application was loaded from, unless the response from other origins includes the right CORS headers. CORS relaxes SOP.

# CORS headers
## Access-Control-Allow-Origin
`Access-Control-Allow-Origin` specifies either a single origin, which tells browsers to allow that origin to access the resource. A wildcard can be specified using `*`, but then allow-credentials will not work. Example:

`Access-Control-Allow-Origin: http://foo.example`

## Access-Control-Allow-Methods
The Access-Control-Allow-Methods header specifies the method or methods allowed when accessing the resource. Example:

Access-Control-Allow-Methods: POST, GET 

## Access-Control-Allow-Headers
The `Access-Control-Allow-Headers` header is used in response to a preflight request to indicate which HTTP headers can be used when making the actual request. Example:

`Access-Control-Allow-Headers: Authorization, Content-Type`  

## Access-Control-Expose-Headers
The `Access-Control-Expose-Headers` header lets a server whitelist headers that browsers are allowed to access. Example:

`Access-Control-Expose-Headers: X-My-Custom-Header` 

## Access-Control-Max-Age
The `Access-Control-Max-Age` header indicates how long the results of a preflight request can be cached. For an example of a preflight request, see the above examples. Example: 

`Access-Control-Max-Age: 86400`

## Access-Control-Allow-Credentials
The `Access-Control-Allow-Credentials` header Indicates whether cookies or http authN information be sent along. Example: 

`Access-Control-Allow-Credentials: true`

# Source attribution
Some parts of this page are based on [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) by Mozilla Contributors, which is licensed under [CC-BY-SA 2.5](http://creativecommons.org/licenses/by-sa/2.5/).