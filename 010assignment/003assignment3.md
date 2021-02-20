# Goal
Create a web application that may only be accessed by authenticated users. While in reality you would likely use external providers (Firebase, Auth0, ...), this is not allowed in this assignment since I want you to understand the complexities of password storage. This means you will need to set up some kind of datastore in this assignment.

In your application, users must be able to sign up with a username/email and password. During signup, you must enforce that the password conforms to the rules set forth by https://pages.nist.gov/800-63-3/sp800-63b.html section 5.1.1.1. 
At least, the [HIBP](https://haveibeenpwned.com/) password list should be used as a blacklist. You must adhere to the secure password storage guidelines as discussed in class.

You will not be graded on the actual functionalities of your application, so simply showing a 'welcome' page after authenticating is fine. 