
# Cyber security principles 
Three of the most well-known cyber security principles are 
* confidentiality
* integrity
* availability. 

These are often referred to as the CIA triad.

# Confidentiality
Confidentiality is about keeping information secret from those who are not allowed to access it. Access management measures such as authentication are examples of an implementation measure aimed to protect confidentiality. Encryption of data is another example.


# Integrity
Integrity should guard against improper information modification or destruction. For example, to be certain that an email is not modified during transport, we could protect its integrity by means of a digital signature. Note that many emails these days are still sent without signing them, therefore increasing the risk of improper modification.

# Availability
Availability measures should ensure the information is timely and reliably accessible. Some information may need an availability up time of 99,97%. Reaching an availability of 100% is nearly impossible. Even Google doesn’t, as you can read here in the extract copied from their [FAQ](https://support.google.com/googlecloud/answer/6056635?hl=en).

![](docimages/2020-02-08-11-29-10.png)

# Two other principles that you will often encounter
## Authenticity
Authenticity relates to the fact that the receiver can be certain the message was sent by a specific sender, and not by some random attacker. 


## Non-repudiation
Non-repudiation is stronger since even third parties can be convinced that the information came from that sender. Should things go to court, the sender will have a hard time denying having sent the message.

> While not part of the course contents, it may be interesting to know that several lists of principles exists, of which the CIA triad is simply the most well-known. Another well-known list are the design principles of Saltzer & Schroeder: http://web.mit.edu/Saltzer/www/publications/protection/Basic.html 