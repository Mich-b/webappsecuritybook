The **same-origin policy** is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.
However, since the web originally had no security boundaries built in, SOP was only added later. This sadly means that SOP does not apply to everything and thus makes it sometimes hard to understand. In what follows, an overview is given of which requests are subject to the SOP and which ones are not. 

# What is cross-origin
Web content's **origin** is defined by the _scheme_ (protocol), _host_ (domain), and _port_ of the URL used to access it. Two objects have the same origin only when the scheme, host, and port all match.

For examples, see https://developer.mozilla.org/en-US/docs/Glossary/origin

# Quick overview of what is allowed and disallowed by the SOP
* Cross-origin _writes_ are typically **allowed**. Examples are links, redirects, and form submissions. However, some HTTP requests do require [preflight](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#Preflighted_requests).
* Cross-origin _embedding_ is typically allowed.
    * JavaScript with `<script src="…"></script>`. Error details for syntax errors are only available for same-origin scripts.
    * CSS applied with `<link rel="stylesheet" href="…">`. 
    * Images displayed by `<img>`. In current browser designs, images loaded like this remain opaque to JavaScript running on the page and cannot trivially be read back by attackers. 
    * Media played by `<video>` and `<audio>`.
    * Plugins embedded with `<object>`, `<embed>`, and `<applet>`.
    * Fonts applied with `@font-face`. Some browsers allow cross-origin fonts, others require same-origin.
    * Anything embedded by `<frame>` and `<iframe>`. Sites can use HTTP headers to prevent cross-origin framing (as we'll see later).
* Cross-origin _reads_ are typically disallowed, but read access is often leaked by embedding. For example, you can read the dimensions of an embedded image, the actions of an embedded script, or the availability of an embedded resource.

The SOP restriction can be **lifted** by using CORS.

> Another excellent resource to learn about SOP is https://code.google.com/archive/p/browsersec/wikis/Part2.wiki#Same-origin_policy

# Source attribution
Some parts of this page are based on [Same-Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) and [Origin](https://developer.mozilla.org/en-US/docs/Glossary/origin) by Mozilla Contributors, which is licensed under [CC-BY-SA 2.5](http://creativecommons.org/licenses/by-sa/2.5/).