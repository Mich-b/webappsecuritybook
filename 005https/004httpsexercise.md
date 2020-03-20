# Exercise
The exercise consists of setting up a HTTPs enabled web server from scratch. 

* Spin up an EC2 Linux micro instance in Amazon Web Services, when you register using your student account, you have gotten free credits (if you have spent all your credits for the assignment, please contact me)
* Link a static public IP address to this instance
* Request a free domain name here: https://www.freenom.com/en/index.html?lang=en
* Configure the Freenom DNS so that it resolves to the static public IP address of your EC2 instance
* Connect to this EC2 instance over SSH
* Install NGINX, a famous web server
* Configure NGINX to listen on your domain name and install [certbot](https://certbot.eff.org/)
* Set up HSTS and check the Certificate Transparency logs
* Check your website against https://www.ssllabs.com/ssltest/ and https://securityheaders.com/ to see if HTTPS was configured correctly

If you are stuck, contact me on Teams or execute a Google search.