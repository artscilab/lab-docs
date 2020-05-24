# Setting up local development for Creative Disturbance

This guide will walk you through setting up a local installation of WordPress, configuring it to be the same as the Creative Disturbance website, and then making contributions to it. There are some important notes on contributing code changes at the bottom. 

## Installing Wordpress

To develop for Creative Disturbance, you'll need to have a working WordPress installation locally. To do this easily, install [MAMP](https://www.mamp.info/en/mamp/mac/) if you're on a Mac, or [WAMP](https://sourceforge.net/projects/wampserver/) if you're on Windows. There exists a version of MAMP for Windows, but that hasn't been tested. 

Get WordPress installed locally. You can download it [here](https://wordpress.org/download/) and follow the instructions to set up a fresh installation on WAMP [here](https://zuziko.com/tutorials/how-to-install-wordpress-on-windows-using-wamp-server/) and on MAMP [here](https://www.wpbeginner.com/wp-tutorials/how-to-install-wordpress-locally-on-mac-using-mamp/).


Once that is done, you should have a file folder structure for the wordpress installation in your WAMP/MAMP directory that's very similar to the one on the server. You'll have a file tree that looks something like this: 

```
installationFolder/
  |- wp-admin/
    |- ...
  |- wp-includes/
    |- ...
  |- wp-content/
    |- gallery/
    |- languages/
    |- themes/
    |- plugins/
    |- uploads/
  |- index.php
  |- license.txt
  |- readme.html
  |- wp-activate.php
  |- wp-blog-header.php
  |- wp-settings.php
  |- wp-signup.php
  |- ...
```

To find out more about the actual organization and what all the files here do, look at [this guide](https://www.wpbeginner.com/beginners-guide/beginners-guide-to-wordpress-file-and-directory-structure/)


## Installing the Creative Disturbance theme

Once the installation on MAMP/WAMP is done, you should be able to access your local installation at some `localhost` URL. The rest of the guide refers to that URL, unless otherwise specified.


Themes live in the `wp-content/themes/` folder. If you open it, you'll see a couple default themes here already. If you go to `Appearance > Themes` in the dashboard, you will see them available for you to Preview or Activate. We are going to do the same for Creative Disturbance.

I use the terminal to do `git` things, so this guide will work with that. You can do the same actions with the Github Desktop app as well. I'm also using `bash` commands (native on *nix systems like the mac), but you should use the Windows equivalent for `cmd` where appropriate. This guide doesn't aim to be a tutorial on Git or the Terminal, but here is a [git cheat sheet](https://education.github.com/git-cheat-sheet-education.pdf) for the basics. 

`cd` into `wp-content/themes` and run the following: 

```bash 
$ git clone https://github.com/artscilab/creativedisturbance.git
```

or if you are using SSH keys to access git: 

```bash 
$ git clone git@github.com:artscilab/creativedisturbance.git
```

Once that's done, you should see a folder called `creativedisturbance` inside `wp-content/themes`, the entire path looks like: `wp-content/themes/creativedisturbance`. Debug if you don't. 

If you refresh the Themes page in the Dashboard, you should now see the Creative Disturbance theme available. Click the button to activate it. 


!> If you're on linux or mac, I highly recommend the [wp-cli](https://wp-cli.org/) utility. It's a command line app that allows you to manage your wordpress site from the command line. If you have it, a simple `wp plugin install pods --activate` will install the pods plugin and activate it all in one go. 

## Setting up Pods

Go to the [Pods plugin page](https://wordpress.org/plugins/pods/) and download it. You'll get a compressed `.zip` file. Extract it and paste the entire "pods" folder inside `wp-content/plugins`. The resulting path for the plugin looks like this: `wp-content/plugins/pods`.

In the Dashboard, on the plugins page, you should now see an option for the Pods plugin. Activate it. 

## Setting up Seriously Simple 

Do the same process that you just did for Pods for the [Seriously Simple](https://wordpress.org/plugins/seriously-simple-podcasting/) plugin.

## Importing the Pods Configuration 

Go to the live site, and in the `Pods Admin` section of the left sidebar, click on `- Migrate Packages`. Migrate Packages is an optional Pods feature that is enabled in order to allow import and export. On this page, click on the big button labeled `Export`, make sure all the boxes are checked, and click on the blue button labeled `Continue`. The page will update with a text box containing all the exported schema data. Leave this page open for now so you can copy it in later. 

Now on the local site, click on the `Components` heading in the Pods Admin section. On this page, scroll to find the row labeled `Migrate: Packages` and hover over it to reveal a link under the title saying "Enable." Click on the link, and the page will reload. Now, do the same for the option labeled `Advanced Relationships`. 

You'll now be able to see and access the same `- Migrate Packages` heading on the local page. Click on that, and this time click on the big button labeled `Import`. Copy the code from the live site, and paste it into the empty text box on this page. Make sure to copy the entire thing. Click the blue button labeled `Continue` to finish up. 

Now, the Pods data types should be set up on your local installation as they are in the live site. You'll see new items in the left sidebar that correspond to the Pods data types that you just imported.

# Pushing code changes

Now, you should be completely set up with everything you need to start developing. You can add Podcasts, Series, Hosts, Voices and everything else on your local installation the same way content admins would be doing on the live site. You can open the `creativedisturbance` folder you cloned in the IDE or editor of your choice, and get going. 

!> Make sure to make [feature branches](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow) for each change, instead of working on the `master` or `dev` branch directly. Any code pushed to `dev` gets deployed to `dev.creativedisturbance.org` and any changes on `master` get pushed to `creativedisturbance.org`. You can see the configuration that defines this behaviour at the [deployer config](https://github.com/artscilab/deployer/blob/master/config.js). 


You should now be on your way to contributing to Creative Disturbance!

---

Contributors: Al Madireddy 