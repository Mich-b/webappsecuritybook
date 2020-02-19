# 'Advanced' authentication
In the previous part about authentication, we mainly talked about how you have to store passwords securely, and how password authentication mechanisms must be protected. 

In modern environments, application developers not often program authentication software themselves anymore. The authentication logic is often decoupled from the applications being developed using federation standards such as SAML and OpenId Connect (OIDC). The authentication logic can then be easily centralized. Also, the use of passwords considered old-fashioned, and many developers like to offer alternative ways of authentication based on something you are or something you have. 

In this chapter, we will talk about these topics:
* Federation standards (with a focus on OpenId Connect)
* Alternative authentication mechanisms
* FIDO2 or the WebAuthn standard

