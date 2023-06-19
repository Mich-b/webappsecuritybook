# XSS prevention: Content security policy
As we've seen in the previous sections, protecting against XSS is hard and requires adherence to a large set of rules, depending on the complexity of your application. 

Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks. A primary goal of CSP is to mitigate and report XSS attacks. XSS attacks exploit the browser's trust of the content received from the server. Malicious scripts are executed by the victim's browser because the browser trusts the source of the content, even when it's not coming from where it seems to be coming from. Configuring Content Security Policy involves adding the Content-Security-Policy HTTP header to a web page and giving it values to control resources the user agent is allowed to load for that page. For example, a page that uploads and displays images could allow images from anywhere, but restrict a form action to a specific endpoint. 

Alternatively, the <meta> element can be used to configure a policy, for example: `<meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-src 'none';">`

# Specifying a Content Security Policy
You can use the Content-Security-Policy HTTP header to specify your policy, like this:

```
Content-Security-Policy: policy
```

The policy is a string containing the policy directives describing your Content Security Policy. A policy is described using a series of policy directives, each of which describes the policy for a certain resource type or policy area. Your policy should include a `default-src` policy directive, which is a fallback for other resource types when they don't have policies of their own. A policy needs to include a `default-src` or `script-src` directive to prevent inline scripts from running, as well as blocking the use of `eval()`. A policy needs to include a `default-src` or `style-src` directive to restrict inline styles from being applied from a `<style>` element or a style attribute.

# Directives
Apart from `default-src`, `script-src`, and `style-src`, other directives exist. A partial list is given below. For a full overview of all directives, [see the Mozilla documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy). Note that some directives prevent against other attacks than XSS alone. 

## connect-src
Restricts the URLs which can be loaded using script interfaces

## default-src
Serves as a fallback for the other fetch directives.

## font-src
Specifies valid sources for fonts loaded using `@font-face`.

## frame-src
Specifies valid sources for nested browsing contexts loading using elements such as `<frame>` and `<iframe>`.

## img-src
Specifies valid sources of images and favicons.

## media-src
Specifies valid sources for loading media using the `<audio>` , `<video>` and `<track>` elements.

## script-src
Specifies valid sources for JavaScript. Two more fine-grained directives exist:

* `script-src-elem`: Specifies valid sources for JavaScript `<script>` elements.
* `script-src-attr`: Specifies valid sources for JavaScript inline event handlers.

## style-src
Specifies valid sources for stylesheets. Two more fine-grained directives exist:

* `style-src-elem`: Specifies valid sources for stylesheets `<style>` elements and `<link>` elements with `rel="stylesheet"`.
* `style-src-attr`: Specifies valid sources for inline styles applied to individual DOM elements.

## form-action
Restricts the URLs which can be used as the target of a form submissions from a given context.

## frame-ancestors
Specifies valid parents that may embed a page using `<frame>`, `<iframe>`, `<object>`, `<embed>`, or `<applet>`.

## navigate-to
Restricts the URLs to which a document can initiate navigation by any means, including `<form>` (if form-action is not specified), `<a>`, `window.location`, `window.open`, etc.

# Values
Each directive can have one or more values. For example, let's consider the following CSP:

```
Content-Security-Policy: default-src 'self'; script-src https://code.jquery.com https://google-analytics.com 'sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8=';
```

This policy says that only scripts from https://code.jquery.com or https://google-analytics.com are allowed, or a script that has `FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8=` as its SHA-256 digest. This digest can be calculated over inline scripts or referenced scripts. 

Typical values are the following:

* `*`: wildcard, allows any URL
* `none`: prevents loading resources from any source.
* `self`: allows loading resources from the same origin (same scheme, host and port).
* `data:`: allows loading resources via the data scheme (eg Base64 encoded images).
* `domain.example.com`: allows loading resources from the specified domain name.
* `*.example.com`: allows loading resources from any subdomain under example.com.
* `https://cdn.com`: allows loading resources only over HTTPS matching the given domain.
* `https:`: allows loading resources only over HTTPS on any domain.
* `unsafe-inline`: allows use of inline source elements. This is ignored when hashes are used.
* `unsafe-eval`: allows unsafe dynamic code evaluation such as JavaScript eval()
* `nonce-`: allows script or style tag to execute if the nonce attribute value matches the header value. Example: `<script nonce="2726c7f26c">alert("hello");</script>`
* `sha256-XXX`: allow a specific script or style to execute if it matches the hash. Example: sha256-qznLcsROx4GACP2dm0UCKCzCG+HiZ1guq6ZZDob/Tng= 
* `strict-dynamic`: propagate trust to all the scripts loaded by that root script. At the same time, any whitelist or source expressions such as 'self' or 'unsafe-inline' will be ignored. 


# Source attribution
Some parts of this page are based on [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) by Mozilla Contributors, which is licensed under [CC-BY-SA 2.5](http://creativecommons.org/licenses/by-sa/2.5/).
