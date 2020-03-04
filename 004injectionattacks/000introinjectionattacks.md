# Injection
Injection attacks are very prevalent, particularly in legacy code. They allow attackers to supply untrusted data to an application, which gets processed as part of a command or query and may alter the foreseen execution flow of that application. 
There are many variants:
* [SQL injection](https://owasp.org/www-project-cheat-sheets/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet)
* [LDAP injection](https://owasp.org/www-project-cheat-sheets/cheatsheets/LDAP_Injection_Prevention_Cheat_Sheet.html)
* [XPath injection](https://owasp.org/www-community/attacks/XPATH_Injection)
* [NoSQL injection](https://erlend.oftedal.no/blog/static-110.html)
* [OS command injection](https://owasp.org/www-community/attacks/Command_Injection)
* [XML parser injection](https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing)
* [SMTP header injection](https://portswigger.net/kb/issues/00200800_smtp-header-injection)
* [expression language injection](https://owasp.org/www-community/vulnerabilities/Expression_Language_Injection)
* [HTML injection](https://www.acunetix.com/vulnerabilities/web/html-injection/)
* [Script injection (XSS)](https://owasp.org/www-community/attacks/xss/)

In this course we will cover SQL injection and command injection briefly. The focus will be put on cross-site scripting or XSS, since that is a vulnerability that is still widespread in modern web applications today. 

# Source attribution
Some parts of this page are based on [OWASP A1 Injection](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection), which is licensed under [FLOSS](https://owasp.org/about/).