# XSS prevention: Context-sensitive output encoding 
Since user input can be inserted in many places, context-sensitive output encoding is not that simple. There are, however, some general rules that can be followed. 

## RULE #0 - Never Insert Untrusted Data 
You should avoid putting untrusted data anywhere. That is rule 0. 

Do not use untrused data directly in a script:

```
<script>...NEVER PUT UNTRUSTED DATA HERE...</script>   
```

Do not use untrused data inside an HTML comment:

```
<html>
<!--...NEVER PUT UNTRUSTED DATA HERE...-->  
</html> 
```

Do not use untrused data in an attribute name:

```
<div ...NEVER PUT UNTRUSTED DATA HERE...=test /> 
```

Do not use untrused data in a tag name:

```
<NEVER PUT UNTRUSTED DATA HERE... href="/test" />
```

Do not use untrused data in a url parameter:

```
<a href="NEVER PUT UNTRUSTED DATA HERE..." />
```

Do not use untrused data directly in CSS:

```
<style>
...NEVER PUT UNTRUSTED DATA HERE...
</style>     
```

If you need to use user input (which you probably do), you must make sure to adhere to rules 1 through 5. 

## RULE #1 - HTML Escape Before Inserting Untrusted Data into HTML Element Content
When using user input in an HTML context, such as inside tags like `div`, `p`, `b`, `td`, etc. you must make sure to HTML encode the output. 

**This rule is typically the only rule that is enforced automatically by frameworks such as Angular and React**.  

### Special case for rule #1: sanitization
Sometimes you may wish to allow users to use an HTML editor. A typical example is a forum where users are allowed to use HTML headers, put text in bold, italic, etc. In that case, HTML encoding would remove all HTML mark-up which may not be desirable. In these cases, we should use HTML _sanitization_. This is more difficult than escaping, and requires the use of HTML sanitization libraries such as [HtmlSanitizer](https://github.com/mganss/HtmlSanitizer) or [DomPurify](https://github.com/cure53/DOMPurify). These libraries can be used in the back-end or can run in the front-end to prevent DOM-based XSS attacks. 

## RULE #2 - Attribute Escape Before Inserting Untrusted Data into HTML Common Attributes
Rule #2 is for putting untrusted data into typical attribute values like width, name, value, etc. 

Except for alphanumeric characters, escape all characters less than 256 with the \xHH (where HH stands for a hexadecimal number, see [ASCII](http://www.asciitable.com/)) format to prevent switching out of the attribute.

Inside unquoted attribute:

```
<div attr=...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...>content
```

Inside single quoted attribute:

```
<div attr='...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...'>content
```

Inside double quoted attribute :

```
<div attr="...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...">content
```

## RULE #3 - JavaScript Escape Before Inserting Untrusted Data into JavaScript Data Values
Rule #3 concerns dynamically generated JavaScript code - both script blocks and event-handler attributes. 

Except for alphanumeric characters, escape all characters less than 256 with the \xHH (where HH stands for a hexadecimal number, see [ASCII](http://www.asciitable.com/)) format to prevent switching out of the data value into the script context or into another attribute. 

Inside a quoted string:

```
<script>alert('...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...')</script>    
```

One side of a quoted expression:

```
<script>x='...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...'</script>
```

Inside quoted event handler:

```
<div onmouseover="x='...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...'"</div>  
```

## RULE #4 - CSS Escape And Strictly Validate Before Inserting Untrusted Data into HTML Style Property Values
Rule #4 is for when you want to put untrusted data into a stylesheet or a style tag.

Except for alphanumeric characters, escape all characters less than 256 with the \xHH (where HH stands for a hexadecimal number, see [ASCII](http://www.asciitable.com/)) format. 

Property value:

```
<style>
selector { property : ...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...; }
</style>
```

```
<style>
selector { property : "...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE..."; }
</style>
```

```
<span style="property : ...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...">text</span>
```

## RULE #5 - URL Escape Before Inserting Untrusted Data into HTML URL Parameter Values
Rule #5 is for when you want to put untrusted data into a query string of an HREF value.

```
<a href="http://www.somesite.com?test=...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...">link</a >  
```

## RULE #6 - Avoid JavaScript URL’s
Untrusted URLs that include the protocol `javascript:` will execute javascript code when used in URL DOM locations such as anchor tag HREF attributes or iFrame src locations. Be sure to validate all untrusted URLs to ensure they only contain safe schemes such as HTTPS.

```
<a href="ONLY ALLOW URIS STARTING WITH THE HTTP OR HTTPS SCHEME">link</a >  
```

This rule is enforced by Angular by default, but was only [very recently introduced in React](https://reactjs.org/blog/2019/08/08/react-v16.9.0.html#deprecating-javascript-urls). Before, React did not by default adhere to this rule. 


# Source attribution
Some parts of this page are based on the [Cross Site Scripting Prevention Cheat Sheet](https://owasp.org/www-project-cheat-sheets/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet), which is licensed under [FLOSS](https://owasp.org/about/).

