# Federation
## Introducing federation
Imagine three applications exist in an enterprise:
* Sales system
* Order system
* AWS

The following would likely be a correct visual representation of the architecture:

![](docimages/2020-02-19-07-31-51.png \| height=100px])

In other words, each application has its own authentication logic (the purple part) and a (shared) user database where passwords are stored. This authentication logic is likely very similar in all three applications. Changes to it would require redundant code changes (for example, adding multi-factor authentication would impact the authentication component of all three applications). 

A much better solution would be to decouple the authentication logic from the rest of the application logic, like so:

![](docimages/2020-02-19-07-39-23.png)

The applications now have to _trust_ the authentication component. The existance of trust is why this pattern is called _federation_. Applications trust a central authentication component, to which they federate the authentication responsibility. 

The _PEP_ on the image is the _Policy Enforcement Point_. This is a generic term for the small part of logic that has to remain in each application to make sure a user went through the central authentication component before trying to access the application. 

## Federation concepts
### Tokens
Typically, when the authenication component is separated from the other applications, security tokens are used to communicate identity information from the authentication component to the other applications. 
![](docimages/2020-02-19-07-50-50.png)

* A security token contains claims about a user
* A claim is a statement that one subject makes about itself or another subject. Claims are issued by a provider, and are given one or more values. Examples of claims:
    * name: michael
    * role: app1_admin, app2_admin
    * email: michael@someapdomain.be

Security tokens can be _self-contained_ or _by reference_. Self-contained tokens have all claims encoded inside the security token, whereas _by reference_ tokens contain a database identifier. An application receiving a _by reference_ token therefore still has to make a round-trip to the authentication component in order to request the claims identified by the reference. _Self-contained_ tokens are most widely used currently. 

### Token formats
Many security token formats exist. The most known format in modern development is the JSON Web Token (JWT). An example of a token is this:
```
 eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL2FwcC50ZXN0LnRrIiwiaXNzIjoiaHR0cHM6Ly9hZGZzLnRlc3QudGsvYWRmcyIsImlhdCI6MTU2NDE1NTQwMiwibmJmIjoxNjY0MTU1NDAyLCJleHAiOjE2NjQxNTkwMDIsInN1YiI6IjUxIiwibmFtZSI6IkthdGUiLCJncm91cCI6ImZvdW5kZXIiLCJ2ZXIiOiIxLjAiLCJzY3AiOiJlbWFpbCBwcm9maWxlIG9wZW5pZCJ9.SNPzlt6Y4bZHkAeMuKTch1pO-1VGGReHXdA9SAOjWgo
```
These tokens can easily be decoded by any Base64 decoding utility. 

Another format, used commonly in traditional environment is a SAML token. These are based on XML and an example is provided here:
```
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
```
This is a bit more difficult to decode, since SAML uses inflation and deflation techniques. An online [SAML decoder](https://www.samltool.com/decode.php) can easily handle this though. 

### Digital signatures
Self-contained tokens **must** be signed by the issuer to protect their authenticity and integrity. Unsigned tokens can easily be modified. Both the JWT as well as the SAML token example provided above were signed. Digital signatures will be covered in more detail later in the course. 

## Protocols to implement federation
Multiple federation protocols exist. Each protocol describes different alternative ways (called flows or grants) to communicate identity information using tokens. Typically, there are only a few alternative ways that are actually being used in practice. 
* SAML 
    * Describes 12 different protocol flows
    * Uses the SAML token format (note that _SAML_ is both a token format and a protocol)
* OAuth2
    * Describes 4 different protocol flows
    * The token format can be freely chosen by the developer, typically JWT
* OpenId Connect
    * Describes 3 different protocol flows
    * Issues two tokens: identity token (JWT) and access token (format can be freely chosen)

We won't cover SAML in this course, but much of the underlying principles that exist in OpenId Connect, were copied over from SAML. 

### OAuth2
What we have called _authentication component_ until now is actually called _Authorization Server_ in OAuth2. This may be confusing: authorization is different from authentication as we've seen in the chapter about basic authentication concepts. 

OAuth2 is actually an authorization protocol, and not an authentication protocol. To make things even more confusing, this protocol was often tailored to make it work for authentication. Tailoring a protocol to make it work for something it was not intended is never a good idea. That is why, pretty soon after its release, the OpenId Connect (OIDC) protocol was invented. OIDC builds further upon OAuth2 and adds an authentication layer on top of it. 

### OpenId Connect (OIDC)
In OpenId Connect (OIDC) the _authentication component_ is called _OpenId Connect Provider_. Since OIDC builds further upon OAuth2, two security tokens are issued:
* identity token
* access token (the OAuth2 part of the flow)

OIDC is mainly used in applications using a JavaScript front-end calling back-end APIs, but may be used in other environments as well. A complete example flow is displayed below. In this example, a JavaScript front-end (the demo app - called _Relying Party_ in OIDC) calls a secured back-end (the API - called _Resource Server_). 
![](docimages/2020-02-19-08-47-57.png)

The flow used in the example is called the _Authorization Code flow_. It is currently best practice to use this flow in favor of its deprecated alternative (the _implicit flow_). Also note that the identity token is used by the relying party, and is **never** used to call an API. Calling an API requires the use of the access token. 

> Refresh tokens, while an important part of the protocol flows discussed, are out of scope for the course. If you want to know more about them, [here's a good start](https://auth0.com/learn/refresh-tokens/). 
> 
## Single Sign On
Single Sign On is defined differently depending on the source you consult, but it basically means that a user has to authenticate once and is then able to use many (if not all) applications without having to authenticate again. 

Federation protocols (such as OIDC) help in creating a Single Sign On (SSO) environment. In case multiple applications trust the same authentication component, a user will only have to authenticate once against that authentication component. 