# Goal
You should develop:
- A Single Page Application
- A REST API
- Set up an OpenID Connect Provider

The goal of the exercise is to add access control (authentication and authorization) to this application using OpenID Connect. You can use any OpenID Connect Provider which you think complies with the requirements of this exercise. 
Ultimately, your SPA must have a button that can be used to fetch a simple string from the REST API. This button is only shown to authenticated users. Only authenticated users that are authorized to call the REST API may be allowed to do so. 

You must use at least two types of tokens: an id token and an access token. The Single Page Application uses the id token of a trusted OpenID Connect Provider to welcome the user and to decide whether or not the button 'Call API' is shown. The REST API must be secured in such a way that it can only be called with an access token that is issued by a trusted OpenID Connect Provider AND that has the scope 'api.read'. 

You must
* Use the authorization code flow
* Use PKCE