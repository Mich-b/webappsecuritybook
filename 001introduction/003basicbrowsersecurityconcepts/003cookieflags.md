# When are cookies sent
The `Domain` and `Path` directives define the scope of the cookie: what URLs the cookies should be sent to. If the domain and path of the cookie match those of the request, the cookie is sent along with the request. 

# Cookie flags
Currently, three flags can be set on cookies:
* Secure
* HttpOnly
* SameSite

## The Secure flag
A secure cookie is only sent to the server with an encrypted request over the HTTPS protocol. Therefore, not only the Domain and Path of the cookie have to match those of the request, but the scheme must do so as well. 

## The HttpOnly flag
To help mitigate cross-site scripting (XSS) attacks, `HttpOnly` cookies are **not** accessible to JavaScript (e.g. through the `Document.cookie` API); they are only sent to the server. XSS will be covered later in the course, and we will then revisit this cookie flag. 


## The SameSite flag
The `SameSite` flag alters the behaviour of when cookies are sent with requests. While they are usually sent along with a request when the `Domain` and `Path` (and optionally scheme) matches, `SameSite` cookies are never sent cross-site (hence the name: SameSite cookie).

The SameSite flag can have three possible values:
* `None` The browser will send cookies with both cross-site requests and same-site requests.
* `Strict` The browser will only send cookies for same-site requests (requests originating from the site that set the cookie). If the request originated from a different URL than the URL of the current location, none of the cookies tagged with the `Strict` attribute will be included.
* `Lax` Same-site cookies are withheld on cross-site subrequests, such as calls to load images or frames, but will be sent when a user navigates to the URL from an external site; for example, by following a link.

Setting this flag provides some protection against cross-site request forgery attacks (CSRF). CSRF will be covered later in the course, and we will then revisit this cookie flag. 

# Source attribution
Some parts of this page are based on [HTTP Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) by Mozilla Contributors, which is licensed under [CC-BY-SA 2.5](http://creativecommons.org/licenses/by-sa/2.5/).