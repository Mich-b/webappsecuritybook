# Goal
You should build a Single Page Application which calls a back-end API to fetch a simple string. The Single Page Application should contain a login button which triggers a multi-factor authentication flow once clicked. After the authentication has been finished successfully, at least two tokens must be present in the Single Page Application: an id token and an access token. The Single Page Application uses the id token to welcome the user (e.g. by displaying the user's name), and the API must be secured so that it can only be called with the access token. 

# Detailed requirements
You should
* Use the authorization code flow
* Use PKCE
* Only return a result from the API when the scope of the access token contains string.read
* Allow new users to register freely

