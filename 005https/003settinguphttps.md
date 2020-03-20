# Setting up HTTPS
Apart from the theoretical concepts shown in the previous sections, there are six actions that you must take to set up a HTTPS enabled web server from scratch:
* Create a keypair and get a certificate
* Configure redirects from HTTP to HTTPS
* Configure your server with adequate cipher suites
* Set the HSTS header
* Set up Certificate Transparency Monitoring

Note that alternative lists exist (such as [this one](https://movingtohttps.com/)). The six steps mentioned here are the most important steps though. 

## Create a keypair and get a certificate
By now you know what a keypair is, and what a certificate is. You also know that a certificate links the public key of your server to the domain name of your server (e.g. example.com). That means you have to find a CA where you can send your public key, and who is prepared to create a certificate linking it to a domain you own. 

Domain ownership is typically validated in one of the following ways:
* the CA will ask you to add a DNS TXT record, proving you are in control of the DNS servers of the domain you are requesting a certificate for
* the CA will ask you to upload a file to your HTTP website, proving you are in control of the web server of the domain you are requesting a certificate for

Many certificate authorities exist, such as Globalsign, Digicert, Let's Encrypt, and others. Let's Encrypt is by far the most user-friendly CA, since scripts and tools exist that can be run on your web server that automatically generate a keypair and do the domain validation for you. The only thing you still have to do is configuring the script.  The most well-known bot that is able to do this generation for you is [Certbot](https://certbot.eff.org/). 

## Configure redirects from HTTP to HTTPS
You cannot disable HTTP completely. This would deny requests from users that first try to hit the HTTP variant of your website (a default of most browser is indeed to first try port 80, the HTTP port). Unless you can be certain that all users will directly try HTTPS, it is better to configure redirects from HTTP to HTTPS. 

## Configure your server with adequate cipher suites
In the previous sections we have seen that HTTPS goes through a certificate verification phase, a key exchange phase, and only then through a bulk encryption phase. All of these phases require different algorithms. The combination of these algorithms is called a _cipher suite_. Examples of cipher suites are the following:
* TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256
* TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256

While we won't go into detail to explain these further, I highly recommend reading [this in-depth guide about cipher suites](https://english.ncsc.nl/publications/publications/2019/juni/01/it-security-guidelines-for-transport-layer-security-tls ). It is simply important to understand that not all cipher suites offer the same protection. Encryption algorithms that were strong in the past, may be broken today. This configuration is often done on web server level, and in some cases even requires some changes to the OS. A great configuration generator for a wide range of web servers can be found here: https://ssl-config.mozilla.org/ , be sure to take a look at it. 

## Set the HSTS header
HTTP Strict Transport Security (HSTS) is a HTTP security header that you can use to tell the browser to immediately connect to the HTTPS port (443) the next time it issues a request. As mentioned in the section about configuring redirects from HTTP to HTTPS, virtually all browsers first try to reach your website over HTTP. By setting the HSTS header, you can instruct the browser to skip trying the HTTP port and immediately try the HTTPS port. This is important to defend against attacks like [SSLStrip](https://moxie.org/software/sslstrip/). 

## Set up Certificate Transparency monitoring
CAs can be hacked (the [Diginotar](https://en.wikipedia.org/wiki/DigiNotar) case). When this happens, the attacker can issue certificates for virtually every domain he or she wishes. Certificate Transparency is a standard which mandates that all certificates that are issued are written to a public log. Owners of web server domains can subscribe to these logs to get notifications when certificates are issued for their domain. 

For example, if you own the domain `www.example.org`, and you have set up Let's Encrypt as your CA, you may become suspicious when you receive a mail that a certificate for the domain `www.example.org` was issued by Globalsign. An example of where such logs can be checked is [crt.sh](https://crt.sh/). 

> An alternative to all steps above is to simply put a proxy in front of your web server. [Cloudflare](https://www.cloudflare.com/ssl/) is a well-known cloud proxy that can be used to simplify HTTPS deployment. However, care must be taken with these set-ups since these proxies only protect the traffic flowing between users of your web server and Cloudflare, not between Cloudflare and your web server. 