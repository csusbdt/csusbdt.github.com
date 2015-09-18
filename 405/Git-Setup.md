---
layout: cse405
---

# Git Setup

## Overview

In this activity you will do the following:

* Install Git on your system if not already installed.
* Generate an SSH key if not already present.
* Email me your SSH public key.

After I get your SSH public key, I will create a remote repository,
which you will use to submit your work in this course.

## Install Git

Before attempting to install Git, see if it's already installed.
Use the following command for this purpose.

    git --version

The lab computers already have Git installed.

If you have a recent version of Xcode installed on a Mac,
then you should already have Git installed.

If git does not appear installed on your system, 
then download the Git installer from [the Git website](http://git-scm.com/) and install.

## Find or generate an SSH public-private key pair

Under Windows, you should run git commands in a Git Bash Shell window,
which is a program that is installed when you install Git.

Under OS X and Linux, you should run git commands in a normal terminal window.

Open a terminal window. (Use Git Bash Shell if you are on Windows.)

Look in your home folder for a hidden folder named _.ssh._

    ls -a ~

The _ls_ command lists the contents of a folder (also called a directory).
The _-a_ argument means list all the contents, including the hidden
files and folders.  The tilda character '~' refers to your home folder.

If you do not find a .ssh folder, then run the following command
to create the folder and generate a public-private key pair within it.
__Do NOT enter a passphrase__ for this command;
just press enter when asked for one (which it does several times).
Also, _accept the default file name and location_, and
replace "Your name" with your name.

    ssh-keygen -t rsa -C "Your name"

The <em>.ssh</em> folder contains the public and private key files
that are used by the ssh client to make secure connections to other hosts.

Your private key is stored in the following location.
Do not send me the private key.

    ~/.ssh/id_rsa

If you do not see this file, run the ssh-keygen command given above to generate it.

You can display the contents of the public key file with the <em>cat</em> command as follows.

    cat ~/.ssh/id_rsa.pub

Your public key should look something like the following.

````
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDymdGr+QUAhvClI/7c8+ulzSxEH783b9tatMlB4ou53YgOTYrsJEN2rLilpgPeM6pxHt3EtD5aVO8boklZmzpwy/eDHSq8Dxzdhv+lxzv8KmRm8wX7vkBgezrQHoBcjWDyiztH/2MoE5uL42yT3goGPBXsbx/rq0QrwUxnzqNMjJ0R2HsWqF5VV/t0G0mJfgZVuCVBokSMmmuKof1KtUk+R0zTlxCMUhc7EMWf39gVXc6+JWJJqthV71VY8mX4y0CSsNa0/ILMIlyUV7kd4OLPi7qwjAlA292tsh+n3McaQAwWIuKJmO6gIq5rAvDsiIXbKQGaoVd4Sb6ABUuMgVo9 David Turner
````

You can distribute your public key, but keep your private key secret.
Other systems will use your public key to create a
cryptographic challenge that only the holder of the private key can solve.
This is called asymmetric key encryption and is the foundation for most
of the secure communication on the Internet.

__IMPORTANT: If you are working under Linux, then run
_ssh-add_ to load your keys into the SSH agent.__

    ssh-add

## Email your public key to me

Copy the contents of id_rsa.pub into the clipboard and paste it
into an email to your instructor.

Under OS X and Linux, you can display the contents of the public key file
with the following; after doing this, copy the text in the terminal window and paste into an email to me.

    cat ~/.ssh/id_rsa.pub

Under Windows, you can copy the contents of the public key file with the following command;
paste this into an email to me.

    clip < ~/.ssh/id_rsa.pub

What you send to me will look like the following.

````
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDymdGr+QUAhvClI/7c8+ulzSxEH783b9tatMlB4ou53YgOTYrsJEN2rLilpgPeM6pxHt3EtD5aVO8boklZmzpwy/eDHSq8Dxzdhv+lxzv8KmRm8wX7vkBgezrQHoBcjWDyiztH/2MoE5uL42yT3goGPBXsbx/rq0QrwUxnzqNMjJ0R2HsWqF5VV/t0G0mJfgZVuCVBokSMmmuKof1KtUk+R0zTlxCMUhc7EMWf39gVXc6+JWJJqthV71VY8mX4y0CSsNa0/ILMIlyUV7kd4OLPi7qwjAlA292tsh+n3McaQAwWIuKJmO6gIq5rAvDsiIXbKQGaoVd4Sb6ABUuMgVo9 turner@turner-retina.home
````

Make sure you include your name in the email that you send to me so I know who you are.

## PROBLEM: Agent admitted failure to sign using the key

If you are working under Linux and receive the message
_Agent admitted failure to sign using the key_, then try running
ssh-add to load your keys into the SSH agent.

    ssh-add

## Multiple computers

If you want to interact with git from more than one machine,
you need to give me the public key for each computer
you plan to work from.

If you use multiple computers to complete work in this course,
remember to start a work session by pulling changes from the
remote repository, and to end a work session
by pushing changes to the remote repository.

To avoid synchronization problems, adhere to the following 2 rules.

- When you start a work session, pull from your remote repository.
- When you end a work session, push to your remote repository.

## Configure Git with your name and email

Before committing your work to your remote repository,
run the following commands to set your name and email.
Use your real name so I can recognize you more easily.

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

You only need to do the above once (for each computer you work on).

## Submitting Your Work

The only thing you need to send me in this assignment is a copy of your ssh public key.
