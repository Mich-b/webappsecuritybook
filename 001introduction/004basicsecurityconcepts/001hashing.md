# Hashes
A cryptographic hash function (CHF) is a mathematical algorithm that maps data of arbitrary size (often called the "message") to a bit string of a fixed size (the "hash value", "hash", or "message digest") and is a one-way function, that is, a function which is practically infeasible to invert.

Ideally, the only way to find a message that produces a given hash is to attempt a brute-force search of possible inputs to see if they produce a match (or use a rainbow table of matched hashes).

The ideal cryptographic hash function has the following main properties:

* it is **deterministic**, meaning that the same message always results in the same hash
* it is **quick** to compute the hash value for any given message
* it is infeasible to generate a message that yields a given hash value
* it is infeasible to find two different messages with the same hash value
* a small change to a message should change the hash value so extensively that the new hash value appears uncorrelated with the old hash value (avalanche effect)

## Examples
* [SHA256Sum of the Kali Linux installer](https://www.kali.org/downloads/)
* The SHA-256 hash of 'Artesis Plantijn' is `b2391de099591f09f4f4f5654fe4c4e55201e8af3027e62186698c9e45a7846c`
* The SHA-256 hash of 'Artesis Plantij**k**' is `4cddeee07650864a4fb27c146e5b20f8d5a14735a14fe3e7aab2c2822813d39d`

## Properties
A cryptographic hash function must be able to withstand all known types of cryptanalytic attack. In theoretical cryptography, the security level of a cryptographic hash function has been defined using the following properties:

* **Pre-image resistance**: 
Given a hash value `b2391de099591f09f4f4f5654fe4c4e55201e8af3027e62186698c9e45a7846c` it should be difficult to find the message `Artesis Plantijn`. This concept is related to that of a one-way function. Functions that lack this property are vulnerable to preimage attacks.
* **Second pre-image resistance**:
Given a message `Artesis Plantijn`, it should be difficult to find a **different** message such that `hash(Artesis Plantijn) = hash(<different message>)`
Functions that lack this property are vulnerable to second-preimage attacks.

* **Collision resistance**:
It should be difficult to find two different messages m1 and m2 such that `hash(m1) = hash(m2)`
Such a pair is called a cryptographic hash collision. 

Weak hashing functions such as MD5 and SHA1 have been proved vulnerable to collision attacks, and sometimes even to second pre-image attacks. Currently, the guideline is to use SHA256 or higher. 

# Source attribution
Some parts of this page are based on [Cryptographic hash function](https://en.wikipedia.org/wiki/Cryptographic_hash_function) by Wikipedia, which is licensed under [CC-BY-SA 3](https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License).