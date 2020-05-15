# Adding a subdomain to atec.io 

This guide aims to help you add a subdomain to atec.io. It involves messing with configuration files on our server, so you'll need ssh access to it. You'll also need access to the lab's DigitalOcean team. Don't attempt this without reading through the guides linked at the bottom of this, and make sure you know what you are doing terminal-wise.

## Step 1
The first step will be to add the name of the desired subdomain as an A-Record on DigitalOcean. 

Follow the following steps:

1. Login to DigitalOcean. You'll need to be added onto the ArtSciLab account. 
2. Use the profile picture button on the top right to switch to the ArtSciLab account.
3. Click on `Networking` on the left-hand sidebar. 
4. In the list of domains, click on the domain name you want to create the subdomain for. I'll use `atec.io` in this guide.
5. Under `Create new record`, find the `Hostname` text input, and enter the subdomain. As you type, you'll see the text under the input change to show you the resulting URL. If you enter "subdomain" it will show you "subdomain.atec.io".
6. Click on the box labeled `Will direct to` and choose the droplet you want it to direct to. Usually it's going to be atec.io. 
7. Leave the TTL as it is, unless you know what you are doing and want to change it. 
8. Click on the blue Create Record button. 
9. That sets up the domain name and lets DigitalOcean know to direct traffic at that address to the server.

## Step 2
The next thing we want to do is create a directory on the server to be the webroot of the project.
First, ssh into the server, and head to `/var/www/`.
If you run an `ls` here, you'll see folders for all the sites that we have. 

Create a new folder for your subdomain using `mkdir`. By default, the new folder will belong to the same group as your username (you can verify this with `ls -al` and in the third and fourth columns, you'll see your username. You want to change the group of the folder to `www-data` so that users in those group can access it. Such a user is the `git` user which has the authorization necessary for running our deploys. 

Change the group of your new folder to `www-data` by running `chgrp www-data <folder name>`. Create another folder inside that called `html` and change it's group to `www-data` as well. That folder is the actual webroot, and is where the static or php website should be served from. This is by lab convention.  

## Step 3
The next thing to do is set up the nginx configuration. We have the configuration on github, at [artscilab/nginx-config](https://github.com/artscilab/nginx-config). 

To set up a config file for the new site, follow these steps:
1. SSH into the server.
2. The nginx config as found at that repo is at `/etc/nginx/`. `cd` there.
3. `cd` into `sites-available/`. This is where you can make new configurations without enabling them yet. (There will later be a symbolic link to a file here from `sites-enabled/` that will enable it)
4. The easiest way to set up a new subdomain config is to copy over one that already exists and change a few things. To set up something like a node.js app, we can use a copy of either `artscilab.atec.io.conf` or `ablb.atec.io.conf` since both of those are node.js reverse-proxies. For this example, I'll use `ablb.atec.io.conf`.

    > A reverse proxy refers to setting up an app as a process running a port, routing traffic from the source to the app, and routing the response back to the client. Read more about it [here](https://www.keycdn.com/support/nginx-reverse-proxy). 
5. Copy `ablb.atec.io.conf` to your new `subdomain.atec.io.conf`. Go inside it and change every occurrence of "ablb" to your new subdomain. 
6. You can see that in this file, the server block contains a couple of `location` blocks which specify specific specific routes that need to handled. The first one, `location /.well-known` points to the ".well-known" folder inside the webroot. This is a special directory that you'll need for the SSL process. Make sure it's pointing to the right webroot. 
7. The second one, `location /` lets all requests go to the app you want to run. Inside the block, change `http://localhost:8001` to the port your app is running on. Choose a port for your app that isn't taken. Currently you'll have to look through all the configs to find one, but that will change soon. For now, there's a lot of numbers out there. 
8. The last block, `location ~ /\.ht` block all requests to anything starting with `ht` or `.ht`.
9. The rest of the server block is pretty self explanatory, just make sure your subdomain is in all the right places instead of the subdomain of the file you copied from. 
10. Because of the next step, wait to change the `ablb.atec.io` in the two lines beginning with `ssl_certificate` and `ssl_certificate_key`. They won't be valid, but the existence of them will let us generate new keys and then we can come back and put in the right path. 

!> Once you're done with these steps, make sure that the config is correct, and still works by running the command `nginx -t`. The `-t` switch runs the test, and the output should tell you whether the new config works, or list any problems with. If anything is wrong, Google is a friend. 

Once it reports no problems with the config, `service nginx restart` will restart the nginx service and the new config will be loaded in. Make sure all the sites are actually online as well, by visiting one of our websites. It's really easy to miss a step and accidentally reload the service on a broken config, which will mean our sites are unaccessible. 

# Step 4
The last step is the SSL certificates for the new subdomain.

We use Let's Encrypt to create our certificates, and the server has their `certbot` utility installed. A read through their documentation is a good idea. 

To begin, you can see the existing certificates when logged in as root, with the command `certbot certificates`. 

To create the certificate for your new subdomain, first run the following command, but put your webroot and domain name where they go. The `-w` flag is followed by the webroot, and the `-d` flag is followed by the domain name. 

```certbot certonly --webroot -w /var/www/ablb.atec.io/html/ -d ablb.atec.io --dry-run```

This only runs a dry run, which is important because the Let's Encrypt service has very low rate limits for actual requests, and the dry run is a way to test our configuration without hitting those rate limits. 

If the dry run doesn't succeed, look up the error and get it fixed. It could be a folder permissions issue (user and group access needs to be correct for the `www-data` group) or it could be something else. 

Once you get the dry-run running successfully, run the same command without the `--dry-run` flag, which will make a real request, and generate the certificate files.

In the output of that command, the utility will show you two pathnames that point to the newly generated certificates. In step 2, we saw two lines in the ablb.atec.io.conf file for the domain that begin with `ssl_certificate` and `ssl_certificate_key`. We didn't change those in the new file for your subdomain, because the new certificate files didn't yet exist. 

We can now go back in and copy the file paths generated by `certbot` into the config on those lines, so that we have the correct ssl certificate paths listed for that subdomain. Go in and change that file, and then run `service nginx restart` once again. Make sure to check for any syntax errors by running `nginx -t` before you restart the service.

If everything goes correctly, then the new subdomain should be successfully set up with a working HTTPS connection. 

---

Contributors:
Al Madireddy 