# Limiting the impact of an XSS attack through the HTTP only flag
In the chapter where the [cookie flags](001introduction\003basicbrowsersecurityconcepts\003cookieflags.md) were explained, we already made mention of the HttpOnly flag. We said the following:

"_To help mitigate cross-site scripting (XSS) attacks, HttpOnly cookies are not accessible to JavaScript (e.g. through the Document.cookie API); they are only sent to the server._"

You should now better understand this statement. If a cookie marked as 'HttpOnly', they cannot be stolen by an attacker even if there is an XSS vulnerability. This flag should not be considered as a fix against XSS, rather it is a 'defense-in-depth' measure to limit the impact of XSS. Cookies that are not expected to be accessed by JavaScript should always be given the HttpOnly flag. 