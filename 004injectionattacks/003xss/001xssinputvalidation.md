# XSS prevention: input validation
Input validation is often considered an adequate protection against XSS, **however it is not enough**. Let's see why. 

Imagine we have the following (pseudo-)HTML code:

```
<!DOCTYPE html>
<html>
<body>

<a href="VARIABLE_1">Your personal website</h1>
<p id="demo">Welcome</p>

VARIABLE_2

<script>
VARIABLE_3
</script>

<style>
p { background-url : "VARIABLE_4"; }
</style>

</body>
</html> 
```

In this example, there are four places where user input can be used (denoted with the word `VARIABLE` in the pseudo code):
* value in an HREF attribute
* in HTML
* in a script
* in CSS

Let's assume user input is collected by means of a form (not shown here) where users can enter their name and website URL as part of a profile page. Upon submitting the form, the data is stored in a database and shown on different places in the application (places starting with `VARIABLE_` in the pseude code above).

# About alert(1)
The payload `<script>alert(1);</script>` is often used by attackers to test if the application is vulnerable to XSS or not. Even though this payload is not harmful (it simply shows a popup), it is easy to replace it with other, more harmful payloads later in the exploitation phase. 

## In HTML
The most common place to use user input is as part of HTML. For example, an application may want to welcome you by using your name 'Welcome Michael'. This requires using user input (`Michael`) in the HTML page. To avoid that an attacker can execute scripts by simply entering `<script>alert('xss')</script>` instead of `Michael` we could choose to validate the input before storing it in a database. 

HTML encoding is an effective encoding technique to prevent injection in HTML. The result of HTML encoding `<script>alert('xss')</script>` is `&lt;script&gt;alert(&#39;xss&#39;)&lt;/script&gt;` . Showing this encoded string in place of `VARIABLE_2` would not be harmful: the encoding was successful in preventing XSS. 

**However, HTML encoding the input does not prevent the other three cases**. 

## HREF value
Let's see what happens if we use HTML encoding to prevent the case of `VARIABLE_1`. The attacker now uses `javascript:alert(1)` as attack payload. HTML encoding this leads to ... `javascript:alert(1)`. It is exactly the same, since no dangerous HTML characters are present in this payload!

Let's now insert that string in lieu of `VARIABLE_1`. Can you imagine what the result would be? HTML encoding the input did not protect us from XSS. 

## In a script
This one is easy. Since I'm already inside script tags, I can simply insert `alert(1)` in place of `VARIABLE_3` and it would be executed. HTML encoding would again not protect us, since this payload does not contain characters that are subject to encoding in HTML. 

## In CSS
Once again, the `javascript:alert(1)` payload would work fine when it replaces `VARIABLE_4`. 
> This CSS injection payload does not work anymore in recent browsers.

# Conclusion
It may be tempting to use input encoding to encode the user's input during submission. However, since it is unknown where this input will be used at the time of submission, input validation is either not enough or very complex to implement. At time of input submission, it is impossible to know that this input will be used as a HREF value, inside HTML, inside a script tag, inside CSS, or in other places. 

It is only known where certain input is shown when this input is rendered to the user (i.e. during output). Therefore, the recommended measure is not input validation, but output validation. Moreover, since the validation rules will differ depending on the context of the output (i.e. HTML, CSS, script, HREF, ...) the output encoding must be context-sensitive, hence the name context-sensitive output encoding. 



