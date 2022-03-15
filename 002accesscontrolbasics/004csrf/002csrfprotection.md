# CSRF protection
For CSRF to work, a couple of conditions must be true:
* the cookie containing the authenticated session identifier is sent along with the request (based on the `scope` of the cookie) - this is default browser behaviour
* the server accepts requests coming from different origins than its own - this is default web behaviour
* the user is authenticated (an authenticated session cookie exists)
* the attacker knows the format and contents of the HTTP request that is being forged

Protection mechanisms typically prevent one of these conditions to be true. Many protection mechanisms against CSRF exist, but we'll discuss three of the most important ones here:
* adopt the synchronizer token pattern
* set the samesite cookie flag
* verify the request origin
* do not use cookies

The first two protection mechanisms are recommended and should be implemented together since the samesite cookie flag is not yet widely supported. The last protection is not recommended but is included for reference. 

## Adopt the synchronizer token pattern
### Without the synchronizer token pattern
The synchronizer token pattern will prevent the attacker from getting to know the exact contents of the HTTP request. 

Let's illustrate the use of the synchronizer token pattern by taking a form submission as example. Note that CSRF does not apply to form submissions only, it applies to all requests that write information to the server. 

An example of a form is as follows:
```
<form action="/action_page.php">
  First name:<br>
  <input type="text" name="firstname" value="Mickey">
  <br>
  Last name:<br>
  <input type="text" name="lastname" value="Mouse">
  <br><br>
  <input type="submit" value="Submit">
</form> 
```

An attacker will copy paste this form and include it on the attacker's site. The only thing that should be changed is the form action parameter, like so:
```
<form action="https://victimwebsite.be/action_page.php">
  First name:<br>
  <input type="text" name="firstname" value="Mickey">
  <br>
  Last name:<br>
  <input type="text" name="lastname" value="Mouse">
  <br><br>
  <input type="submit" value="Submit">
</form> 
```
Since the form will be hosted on the attacker's website, we must explicitly tell the form to submit to the website where the form was copied from (the website we are attacking). 

All the attacker then has to do is automatically submit the form using some JavaScript as soon as a victim opens the attacker's website. 

### With the synchronizer token pattern
The synchronizer token pattern will prevent the attacker from being able to copy the form by including a unique, random string on the original form:
```
<form action="/action_page.php">
  First name:<br>
  <input type="text" name="firstname" value="Mickey">
  <br>
  Last name:<br>
  <input type="text" name="lastname" value="Mouse">
  <br><br>
  <input type="hidden" name="anticsrftoken" value="PErJSCFYqUCmMQd0IM0mzbbwK9uJWwoVg">
  <input type="submit" value="Submit">
</form> 
```
The anti-CSRF token in the form above has two important properties:
* it is tied to the authenticated user
* it is long & random

An attacker browsing to that same form to copy it in preparation of the attack will receive a different anti-CSRF token, one that's linked to the attacker's account. When the victim unwillingly submits the attacker's form, the server verification of the anti-CSRF token will **fail since the victim's submission contains the attacker's CSRF token**, which is not allowed.

## Set the samesite cookie flag
> Before proceeding, please read and understand the [cookie](../../001introduction\003basicbrowsersecurityconcepts\003cookies.md) section of the basic browser security concepts section. That section explains the SameSite cookie flag in detail. 

As you know, a browser's decision to attach a cookie to the request is based on that cookies' scope, made up of the domain and path (and optionally the scheme). If the scope of the request equals the scope of the cookie, the cookie is attached. Browsers don't care about the origin of the request, they only care about the cookie's scope and the request's scope. 

However, marking a cookie as `SameSite=Lax` or `SameSite=Strict` alters that behaviour and prevents that cookie from being sent with cross-origin requests.

This prevents a CSRF attack from succeeding: when the cookie is not attached, the problem of CSRF does not exist since the server that is being attacked will simply discard the request because it is not authenticated. 

## Verify the request origin
**Using this mechanism is not recommended.**

Almost all browsers add an `Origin` or `Referrer` header to POST, PUT, and DELETE requests:

```
GET https://www.w3schools.com/action_page.php?firstname=Mickey&lastname=Mouse HTTP/1.1
Host: www.w3schools.com
Referer: https://www.w3schools.com/html/tryit.asp?filename=tryhtml_form_submit
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,nl;q=0.8
```
Servers can use these headers to determine whether or not the request came from a trusted source. 

This mechanism is not recommended since `Origin` and `Referrer` header are not always included and attackers have been able to forge these headers in the past. 

## Do not use cookies 
This protection mechanism is mentioned last, even though it is the simplest one to implement: simply do not use a cookie to store a user's session. In practice however, cookies are very often used for exactly that purpose. 

It is only in recent years that some applications take a pure asynchronous API-based approach where the front-end interacts with the back-end using API calls only. Surely these API calls must also be authenticated and authorized, but most of the time that is done by adding a token to an HTTP request header (such as the 'Authorization' request header). In such a setup, sometimes no cookies are used and therefore no risk for CSRF exists. 
