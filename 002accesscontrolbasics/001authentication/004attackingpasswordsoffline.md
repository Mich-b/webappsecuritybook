# Attacking passwords - offline
Apart from attempting brute-force attacks against live systems, attackers may find vulnerabilities in applications allowing them to download a copy of the full user-password database table over to their local machine. Since the attacker now has access to the passwords, even when not being online, this is the 'offline' variant of a password attack.

Most defensive measures against brute-force attacks discussed before occur at the logic tier, therefore they will not be very useful against an attacker that is able to obtain a copy of the database. However, when passwords are stored securely, we can still prevent attacker from getting the password even if the datastore is obtained.

# Securely storing passwords
> Make sure you have read and understood the section about [hashing](001introduction\004basicsecurityconcepts\001hashing.md). 

## Attempt 1 - store passwords plaintext
Obviously, when the passwords are stored plain-text in the database, it is trivial for the attacker to extract them. The passwords are now known to the attacker and can be used to login or to reuse them against other services. 

## Attempt 2 - hash passwords
As we've seen in the section about [hashing](001introduction\004basicsecurityconcepts\001hashing.md), it is infeasible to go back from a hash to the original message. Therefore, if we store the password using a hash, let's say SHA256, we are safe right? 

As we've seen in the section about hashing, 'the only way to find a message that produces a given hash is to attempt a brute-force search of possible inputs to see if they produce a match'. Now, if your input to the hash function (the password) is unique, a brute-force search is infeasible. However, if your input (the password) is easily guessable, the hash is worth nothing. 

Let's see how that would work using an example. 
* Assume you have 1000 user accounts, 500 of which have chosen `123456` as their password. 
* The SHA256 result of `123456` is `8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92`, this means that there will be 500 entries in the database with that hash value. 
* The attacker may now take a look at the [top 100 password list](https://en.wikipedia.org/wiki/List_of_the_most_common_passwords), and calculate the hash value of all top 100 passwords.
    * `password` => `5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8`
    * `123456` => `8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92`
    * `12345678` => `ef797c8118f02dfb649607dd5d3f8c7623048c9c063d532cc95c5ed7a898a64f`
    * etc.
* He compares this list of calculated hashes against the list of hashes in the breached databases. Bingo, `8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92` matches `123456`

Hashing passwords using a common hashing function such as SHA256 will only safeguard users that have chosen strong, unique passwords. However, many users choose weak passwords which occur in common breach or dictionary lists. 

## Attempt 3 - hash and salt passwords
Adding a salt to a password means adding random bytes to the password before hashing it. Before storing the password `123456`, the application would first add some random bytes, for example `!qs78=` to the password. The full password now has become `123456!qs78=`. Hashing this value leads to `e29b2f9fca3e45aba4346b9f6a2df0a1f063d23daf1a686c2698a314462bcb9f`, which is completely different from the hash result of `123456`. 

If we create a unique salt per user, the resulting hashes for the 500 users using the password `123456` would still all be completely different! That's a good thing. 

However, these salts have to be stored somewhere, since the user will still use `123456` during authentication. Typically salts are stored in the database... which is breached. We should assume the attacker has access to:
* the salt (`!qs78=`)
* the hash of the password+salt (`e29b2f9fca3e45aba4346b9f6a2df0a1f063d23daf1a686c2698a314462bcb9f`)

A persistent attacker would now slightly modify the attacking procedure discussed in the previous section. Instead of calculating the hashes of the top 100 password list, he or she would now first append the salt, and then calculate the hashes. **This slows down the attacker since this process has to be repeated for every password hash in the database** (since each user is given a unique salt). Still, calculating these hashes with modern hardware is extremely fast as shown during a demo in class. Using common hashing functions with a salt is therefore only a small improvement. 

## Attempt 4 - salt and slow hashing function
We should try to improve on the previous situation by significantly slowing down the attacker in his or her brute forcing attempts. This is possible using slow hashing functions, hash functions that were invented for the purpose of being slow (by consuming a lot of resources). 

Common examples of slow hashing functions are:
* PBKDF2
* Bcrypt
* Scrypt
* Argon-2

Always use one of these functions for password storage. 

> For those looking for a more in-depth coverage of the storing options we just discussed, take a look at this article: https://crackstation.net/hashing-security.htm . 