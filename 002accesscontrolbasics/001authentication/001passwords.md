# Weak passwords
Something you are mechanisms such as passwords are among the most used authentication mechanisms out there. The reason they are is because passwords are often created by humans and must be remembered by humans, resulting in weak passwords. 

# Password guidelines
To prevent attackers from simply guessing user's passwords, password guidelines were created that introduced complexity rules on passwords. The idea was to increase the unpredictability of a password (entropy), and thus making it less likely to be cracked by a bruteforce attack (see later). Composition rules were amongst the most common guidelines: you had to use at least one digit and symbol and capital and lowercase character. However, analyses of breached password databases have shown that the benefit of these rules is not as significant as we initially thought. 

Therefore, [NIST has released a new version of the guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html#memsecret) which explicitly discourages the use of composition rules to determine a password's strength. Rather, they provide two simple rules that application owners should implement:
* Require users to choose a password of at least 8 characters
* Compare the chosen password against a list that contains values to be commonly used, expected, or compromised. This list may include:
    * Passwords obtained from previous breaches (e.g. https://haveibeenpwned.com/)
    * Dictionary words
    * Repetitive or sequential characters
    * Context-specific words (service name, username, etc.)

That's it. 

# Commonly used or compromised passwords
While the guidelines have become easier, the implementation of the last requirement (comparing the password) is relatively new and may require some extra development work. The '[have i been pwned (HIBP)](https://haveibeenpwned.com/)' database is among the most well-known publicly accessible databases containing compromised passwords. Users can manually check if their account is present in breaches (of course, only breach sources that [are publicly available](https://haveibeenpwned.com/PwnedWebsites) are included in HIBP). 

However, more interestingly, HIBP provides an API that can be used to programmatically query a password to check if it is present in the HIBP database. This allows system administrators and application developers to easily fulfill the second NIST requirement. Of course, the HIBP list should be complemented with other lists such as dictionaries and top 100 password lists.

> Some people may wonder if it's a good idea to send passwords to HIBP. The maintainer of HIBP did a great job, and passwords should actually not be sent in plain text to the HIBP API. Rather, searches are carried out using the first 5 characters of the hashed version of the password. This may result in false positives, but it also prevents the HIBP API servers from knowing which exact password is being tested. For those interested, more information can be [found here](https://www.troyhunt.com/ive-just-launched-pwned-passwords-version-2/). 

