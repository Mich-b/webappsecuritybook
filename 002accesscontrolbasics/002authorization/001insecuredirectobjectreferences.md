# Insecure direct object reference
Insecure Direct Object Reference (called IDOR from here) occurs when a developer exposes a reference to an internal implementation object, such as a file, directory, database record, or key, as a URL or form parameter. Without validating this user supplied input, direct access to the object requested may be provided even if the user should does not have authorization to view or alter that object.

![](docimages/2020-02-12-09-28-19.png)

IDOR should be prevented by enforcing adequate authorization policies such that users cannot act outside of their intended permissions

# Source attribution
Some parts of this page are based on the [Insecure Direct Object Reference Prevention Cheat Sheet by OWASP](https://owasp.org/www-project-cheat-sheets/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet) and the [IDOR presentation](https://owasp.org/www-chapter-ghana/assets/slides/IDOR.pdf), which is licensed under [FLOSS](https://owasp.org/about/).