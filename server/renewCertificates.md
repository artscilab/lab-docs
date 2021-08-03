# Renew Cerificates

If server certifcates expire, you will get the `NET::ERR_CERT_DATE_INVALID` error when you try to visit the website in the browser.

We use Let's Encrypt to create our certificates, and the server has their `certbot` utility installed. A read through their documentation is a good idea. 

To begin, you can see the existing certificates when logged in as root, with the command `certbot certificates`. 

To renew all the certificates, use `sudo certbot renew` command.