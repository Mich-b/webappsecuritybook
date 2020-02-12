# Session management - hijacking
As covered in the section about [http](../001introduction\002basicwebconcepts\001http.md) and [cookies](../001introduction\002basicwebconcepts\003cookies.md), the HTTP protocol is stateless. However, developers can implement session management capabilities into their application to tie HTTP requests and responses together in one single session. 

Most often, this is done by setting a session cookie which contains a unique session identifier. When used in conjunction with access control, the session identifier binds the user authentication credentials to the user's HTTP traffic. This way, the application can not only tie HTTP sessions together, but can it also know which identity is executing the actions. 

Common examples of session identifiers in cookies are cookies with the name `JSESSIONID`, `PHPSESSIONID`, or `ASP.NET_SessionId`

# Session identifier requirements
* A session identifier should not be descriptive nor offer unnecessary details about the purpose and meaning of the ID
* A session identifier should be long enough to prevent bruteforce attacks
* A session identifier must be random enough to prevent guessing attacks
* A session identifier must be meaningless to prevent information disclosure attacks
* It is essential to use an encrypted HTTPS connection for the entire web session, to prevent the session identifier from leaking
* The cookie containing the session identifier should be marked as 'secure' to prevent it from being transmitted over insecure HTTP connections
* The cookie containing the session identifier should be marked as 'httponly' to not allow scripts access to it in the case of XSS (this cookie flag will be discussed in more detailed in the XSS chapter)
* The cookie containing the session identifier may be marked as 'samesite', but be sure to understand the potential impact setting this flag may have (this cookie flag will be discussed in more detail in the CSRF chapter)

Most web development frameworks such as J2EE, ASP.NET, PHP, and other provide their own session management features and associated implementation. It is recommended to use these built-in frameworks versus building a home made one from scratch. 


# Source attribution
Some parts of this page are based on the [Session Management Cheat Sheet](https://owasp.org/www-project-cheat-sheets/cheatsheets/Session_Management_Cheat_Sheet) by OWASP, which is licensed under [FLOSS](https://owasp.org/about/).