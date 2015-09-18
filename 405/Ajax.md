---
layout: cse405
---

# Ajax

## Overview

The purpose of this assignment is to learn how to retrieve data from the server using ajax.

## Assignment folder

Create a directory named ajax for the work you do for this assignment. At the end of the assignment, this folder will contain the following files.

* app.html
* image.js
* main.js
* message.js
* root.js
* image.png

## Instructions

You should start the assignment by making a copy of all the files from the file assignment. This will be the starting point for your work.

Read up to and including step 3 of [Ajax - Getting Started](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started). Test the example code presented in step 3. Do this by adding the code to app.html. However, instead of requesting test.html as in the example code, request a resource that is retrievable through the path `/message`.

In the server code, return the string "hello ajax" for requests for `/message`. Do this in a module named message.js. The message module should resemble the root module provided in the buffers assignment because we simply need to return a string of "content-type text/plain; charset=UTF-8".

No init function is needed for the message module.

## Video

* [Recorded presentation on the Ajax Assignment](http://youtu.be/jWCkdjIUZa4)

## Extra Reading

An important topic not dealt with by the sample code in this course is
_cross-origin resource sharing (CORS)_.  A good explanation of this
is on the Mozilla Developer Network website at
[HTTP access control (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS).

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _Ajax assignment_ in the subject line of the email.

