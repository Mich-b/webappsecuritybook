# Goal
Create a web application where users can register themselves with a username and password. Make sure the password conforms to the rules set forth by https://pages.nist.gov/800-63-3/sp800-63b.html section 5.1.1.1. 
At least, the [HIBP](https://haveibeenpwned.com/) password list should be used as a blacklist. Adhere to the secure password guidelines as discussed in class. 

Using black-box breached password mechanisms of vendors is not allowed. For example, using [this](https://auth0.com/breached-passwords) is not allowed. Although in practice this may be a very good option, the goal of this exercise to teach you how to handle requests like this when no out of the box solutions are available. 