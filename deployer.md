# The ArtSciLab Deployer 

The deployer (this repo) is an automated deploy utility.

The basic idea is that it receives a notification from GitHub via webhooks (details below) whenever there are new changes to a codebase. It then reads deployment configuration details from config.js and carries out the update accordingly. 

All details about all the different services are contained within config.js, which allows us to follow a centralized approach to this, and make/track changes to projects in one place.

### Deployment details
The deployer itself is deployed on the ArtSciLab Heroku (consult the ArtSci Heroku wiki page if it exists).

### Configuring a new deployment
There are two ways to configure deployment: deploy using git pull or delete everything and then download and extract the latest archive from github. As you can imagine, the git pull way is preferred. 

If you'd like to register a project to use the automated deployer, follow the following steps: 
* Log onto the server and use `git clone` to get the project on there. Use a location like your home folder 
* Run whatever scripts to build it for the first time
* Use `chmod` and `chgrp` to assign the `www-data` group to that project and give read+write+execute permissions to the group.
* Edit `config.js` in the root of this project to reflect the new project and fill in the details. Look at existing projects as a guide. 
* Go to the settings page of the repo you are setting up deployments for. 
* Click on the webhooks tab in the left menu, and then Add Webhook. 
* Paste in the following URL (correct as of the time of writing this wiki): https://artsci-deploy.herokuapp.com/webhook.
* Make sure `just the push event` is marked, and check the box to enable `SSL verification`. Leave `secret` empty.
* Click on Add Webhook to finish up. 

Now, every push to that repository will trigger a webhook. Github will sent a POST request to the deployer app at that URL you just pasted. The deployer app will follow the logic in `index.js` to carry out deployment. 
