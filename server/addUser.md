# Adding a new user to the server

This guide walk you through adding a new user to the server. 

You'll also need to have sudo access to the server already in order to do it. Log into the server and follow the steps below. 


## Part 1: Creating the user

1.  Create a new user
  
  ```bash
  $ sudo adduser newuser
  ```

  Use a temporary (but still secure) password, and let the person know what it is. They can then log in and change the password to something only they know. 

2. Add the user to the `sudo` group
  
  ```bash
  $ usermod -aG sudo newuser
  ```

3. Add the user to the www-data group

  ```bash
  $ usermod -aG www-data newuser
  ```

4. Test
  First switch to the new user, then run a command as root 

  ```bash
    $ su - newuser
    $ sudo ls -al /root
  ```

## Part 2: Adding SSH accesss

1. Switch to the new user 

  ```bash
  $ su - newuser
  ```

2. Create the hidden `.ssh` folder in their home directory

  ```bash
  $ mkdir ~/.ssh
  ```

3. Create a text file called `authorized_keys`

  ```bash
  $ nano ~/.ssh/authorized_keys
  ```

  > Nano is a terminal-based editor. It offers simple functionality and is suitable for simple tasks such as this one. For other things we do on the server, like working with configuration files and such, I recommend learning an editor like vim or emacs. 

4. Get a Public Key file from the user, they can generate one on their machine according to the instructions found here: [Generating a new SSH key](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

5. Paste in the contents of the user's public key into the `authorized_keys` file, and Save it. `Ctrl`+`x` will make nano prompt to save and then close the file.

6. Once that is done, the user should be able to log into an SSH session with: 
  ```bash
  $ ssh newuser@atec.io
  ```

  They may optional need to pass in the `-i` flag, which will allow them to specify a private key file, in the case that they're not using the same one their system is set up to use by default. That will look like this: 
  
  ```bash
  $ ssh newuser@atec.io -i /path/to/private/key/file
  ```

!> Make sure that once the user logs in for the first time via ssh, they reset their password with the simple command: `passwd`

---

Contributed by: Al Madireddy