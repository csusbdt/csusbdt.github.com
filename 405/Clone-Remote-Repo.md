---
layout: cse405
---

# Clone Remote Repository

## Overview

In this course, you will submit work by pushing your source code into a private remote Git repository that I set up for you.  In order for me to set up a private remote repository for you, you need to send me the SSH public key that your Git client program uses when connecting to remote hosts.  This is what you did in the first activity of this course.

After you send send me your public key, I will create an empty remote repository for you to push commits into.
I will then send you a Git command to use to create a local repository by cloning your remote repository.
You will be able to clone on any number of computers and within any number of locations on a given computer.

If you have not received your clone command yet, wait for me to send it to you.  If you don't get it within a day or two, then send me a reminder.

## Local repository assumption

In the course web pages, I assume that you have cloned your remote repository into a folder named "405" that is located in your home directory. In the instructions, I may refer to this folder with the path `~/405`. The tilde character `~` designates your home directory. (Note that the terms "directory" and "folder" mean the same thing.)

The folder `~/405` is called a _working folder_ because it contains a subfolder named `.git` -- which is actually your local repository -- along with the files and folders that comprise the branch you have currently checked out.

## Pushing Changes

Each assignment normally asks you to create a new folder within your repository to store the files associated with the assignment.  After completing the assignment, you should add and commit new files into your local repository and then push changes to your remote repository.

__How do I know that my push operation worked?__

It is simple to check that your push operation worked.
Make a new temporary directory somewhere in your file system
but not inside your working folder.  Then run the clone command again.
What you get from this clone operation is what is currently in your remote repository.

You can delete this temporary clone of your repository after you are finished checking its contents.

## Submitting Your Work

You do not need to submit anything for this assignment.
