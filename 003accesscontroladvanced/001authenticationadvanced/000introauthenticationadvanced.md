# 'Advanced' authentication
In the previous part about authentication, we mainly talked about how you have to store passwords securely, and how password authentication mechanisms must be protected. 

In modern environments, application developers not often program authentication software themselves anymore. The authentication logic is often decoupled from the applications being developed using federation standards such as SAML and OpenId Connect (OIDC). The authentication logic can then be easily centralized. 

Secondly, the use of passwords considered old-fashioned, and many clients like to offer alternative ways of authentication based on something you are or something you have. In reality though, the password is still ubiquitous these days. A possible reason for this fact is provided by Joseph Bonneau, Cormac Herley, Paul C. van Oorschot, and Frank Stajano. In a [2012 paper](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-817.pdf) they compare 36 authentication methods on three scales:
* usability: how easy it is for a user to use the authenticator
* deployability: how easy it is for a server to deploy the mechanism, including compatibility considerations
* security: how secure the authenticator is

The result was that some mechanisms are more usable than passwords, some are more secure, but there is **not one other mechanism that is more deployable than passwords**. 

In this chapter, we will therefore talk about these topics:
* Federation standards (with a focus on OpenId Connect)
* Alternative authentication mechanisms
* FIDO2 or the WebAuthn standard

