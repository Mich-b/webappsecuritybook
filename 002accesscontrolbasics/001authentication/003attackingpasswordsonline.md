# Attacking passwords - online
There are a number of different types of automated attacks that attackers can use to try and compromise user accounts. The most common types are listed below:

| Attack Type | Description |
|-------------|-------------|
| Brute Force | Testing multiple passwords from dictionary or other source against a single account. |
| Credential Stuffing | Testing username/password pairs obtained from the breach of another site. |
| Password Spraying | Testing a single weak password against a large number of different accounts.|

Different protection mechanisms can be implemented to protect against these attacks. In many cases these defences do not provide complete protection, but when a number of them are implemented in a defence-in-depth approach, a reasonable level of protection can be achieved.

The following sections will focus primarily on preventing brute-force attacks, although these controls can also be effective against other types of attacks. 

## Use strong passwords
See the [section about password guidelines](../../002accesscontrolbasics\001authentication\001passwords.md).

## Multi-Factor Authentication
Multi-factor authentication (MFA) is by far the best defense against the majority of password-related attacks, including brute-force attacks, with analysis by Microsoft suggesting that it would have stopped 99.9% of account compromises. 

## Account Lockout
The most common protection against brute force attacks is to implement account lockout, which prevents any more login attempts for a period after a certain number of failed logins.

The counter of failed logins should be associated with the account itself, rather than the source IP address, in order to prevent an attacker making login attempts from a large number of different IP addresses. There are a number of different factors that should be considered when implementing an account lockout policy in order to find a balance between security and usability:

* The number of failed attempts before the account is locked out (lockout threshold).
* The time period that these attempts must occur within (observation window).
* How long the account is locked out for (lockout duration).

However, [numerous reasons exist](https://www.owasp.org/index.php/Blocking_Brute_Force_Attacks#Locking_Accounts) to think twice about implementing such a mechanism. Think about how easy it would be to block someone from accessing their Facebook account: just attempt to login using their account and they will be locked. Lock-out mechanisms lead to a very high risk of making the application unavailable. 

## CAPTCHA
The use of an effective CAPTCHA can help to prevent automated login attempts against accounts. However, many CAPTCHA implementations have weaknesses that allow them to be solved using automated techniques, or can be outsourced to [services which can solve them](https://www.deathbycaptcha.com/user/login). 

It may be more user-friendly to only require a CAPTCHA be solved after a small number of failed login attempts, rather than requiring it from the very first login.

## Logging and Monitoring
Logging is one of the most important detective controls that application developers should implement. Since it is likely that a breach will occur some day, logs must be kept to help identifying the nature of the breach. 

* Ensure that all failures are logged and reviewed
* Ensure that all password failures are logged and reviewed
* Ensure that all account lockouts are logged and reviewed

# Source attribution
Some parts of this page are based on the [OWASP Authentication cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html), which is licensed under [FLOSS](https://owasp.org/about/).