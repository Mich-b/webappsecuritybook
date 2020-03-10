# HTTP headers for security
We have already covered the use of two security headers: `Content Security Policy` and `Strict-Transport-Security`. We could use these to tell the browser in detail what was and what was not allowed. Headers are _configured_ by the application administrator (since they are response headers) but are _enforced_ by the browser. 

Other HTTP headers which are designed for security exist. They are very easy to configure and are a quick win in most cases. 

## X-Frame-Options
`X-Frame-Options` is the predecessor of the `frame-ancestors` directive of `Content Security Policy`. It can be used to indicate whether or not a browser should be allowed to render a page in a `<frame>`, `<iframe>`, `<embed>` or `<object>`. Sites can use this to avoid clickjacking attacks, by ensuring that their content is not embedded into other sites.

There are two possible directives for this header:
* `X-Frame-Options: deny`
* `X-Frame-Options: sameorigin`

If you specify deny, not only will attempts to load the page in a frame fail when loaded from other sites, attempts to do so will even fail when loaded from the same site. On the other hand, if you specify sameorigin, you can still use the page in a frame as long as the site including it in a frame is the same as the one serving the page.

## X-XSS-Protection
The HTTP `X-XSS-Protection` response header is a feature of Internet Explorer, Chrome and Safari that stops pages from loading when they detect reflected cross-site scripting (XSS) attacks. Although these protections are largely unnecessary in modern browsers when sites implement a strong `Content-Security-Policy` that disables the use of inline JavaScript ('unsafe-inline'), they can still provide protections for users of older web browsers that don't yet support CSP.

There are four possible directives for this header, of which three are shown below:
* `X-XSS-Protection: 0`
* `X-XSS-Protection: 1` (usually the default)
* `X-XSS-Protection: 1; mode=block`

## X-Content-Type-Options
The `X-Content-Type-Options` response HTTP header is a marker used by the server to indicate that the MIME types advertised in the `Content-Type` headers should not be changed and be followed. This allows to opt-out of MIME type sniffing, or, in other words, it is a way to say that the webmasters knew what they were doing.

This header was introduced by Microsoft in IE 8 as a way for webmasters to block content sniffing that was happening and could transform non-executable MIME types into executable MIME types. Since then, other browsers have introduced it, even if their MIME sniffing algorithms were less aggressive.

There is only one way this header can be configured:
`X-Content-Type-Options: nosniff`

## Referrer-Policy
The `Referrer-Policy` HTTP header controls how much referrer information (sent via the `Referer` header) should be included with requests.

This header has 8 directives:
* `Referrer-Policy: no-referrer`: The Referer header will be omitted entirely. No referrer information is sent along with requests.
* `Referrer-Policy: no-referrer-when-downgrade`: This is the default behavior if no policy is specified, or if the provided value is invalid. The origin, path, and querystring of the URL are sent as a referrer when the protocol security level stays the same (HTTP→HTTP, HTTPS→HTTPS) or improves (HTTP→HTTPS), but isn't sent to less secure destinations (HTTPS→HTTP).
* `Referrer-Policy: origin`: Only send the origin of the document as the referrer. For example, a document at https://example.com/page.html will send the referrer https://example.com/.
* `Referrer-Policy: origin-when-cross-origin`: Send the origin, path, and query string when performing a same-origin request, but only send the origin of the document for other cases.
* `Referrer-Policy: same-origin`: A referrer will be sent for same-site origins, but cross-origin requests will send no referrer information.
* `Referrer-Policy: strict-origin`: Only send the origin of the document as the referrer when the protocol security level stays the same (HTTPS→HTTPS), but don't send it to a less secure destination (HTTPS→HTTP).
* `Referrer-Policy: strict-origin-when-cross-origin`: Send the origin, path, and querystring when performing a same-origin request, only send the origin when the protocol security level stays the same while performing a cross-origin request (HTTPS→HTTPS), and send no header to any less-secure destinations (HTTPS→HTTP).
* `Referrer-Policy: unsafe-url`: Send the origin, path, and query string when performing any request, regardless of security.

## Feature-Policy
The HTTP `Feature-Policy` header provides a mechanism to allow and deny the use of browser features in its own frame, and in content within any `<iframe>` elements in the document. For example, you can use this header if you want to control whether a document (a web page) is allowed to access the user's camera device. 

> Note that usually, the user has to provide permission before it can be used (even if the feature is allowed by the `feature-policy` header). 
 
You can send the `Feature-Policy` HTTP header with the response of a page. The value of this header is a policy to be enforced by the browser for the given page. It has the following structure:
`Feature-Policy: <directive> <allowlist>`

For example, to block all content from using the Geolocation API across your site:
`Feature-Policy: geolocation 'none'`

Many features exist, and thus can be blocked by this header. These include the following (non-exhaustive):
* accelerometer
* camera
* fullscreen
* gyroscope
* microphone

For a full overview, please see https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Feature-Policy . 

## Check your headers
You can run your website through https://securityheaders.com/ to see how your website is doing with regards to HTTP Security Headers. Make sure you score an A or better!


# Source attribution
Some parts of this page are based on [X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options), [X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection), [X-Content-Type-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options), [Referrer-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy) and [Using Feature Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Feature_Policy/Using_Feature_Policy) by Mozilla Contributors, which are licensed under [CC-BY-SA 2.5](http://creativecommons.org/licenses/by-sa/2.5/).
