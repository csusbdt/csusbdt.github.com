---
layout: cse405
---

# JSON

## Overview

The purpose of this assignment is to learn how to return ajax data in the JSON format.

## Assignment folder

Create a directory named json for the work you do for this assignment. At the end of the assignment, this folder will contain the following files.

* app.html
* main.js
* root.js
* message.js
* image.js
* image.png

## Instructions

You should start the assignment by making a copy of all the files from the Ajax assignment. This will be the starting point for your experiments.

Read about [the JSON data representation format](http://json.org/).

Modify message.js so that it returns the message data in the following JSON format.

    { "msg": "Hello JSON." }

The following code shows one way to do this.

~~~~
var responseObject = { msg: 'Hello JSON.' };
var responseString = JSON.stringify(responseObject);
var body = new Buffer(responseString, 'utf-8');
~~~~

Change the content-type header in the reply message to application/json without specifying charset.

You should omit the charset specification when returning JSON data; see [this Stackoverflow discussion](http://stackoverflow.com/questions/13096259/can-charset-parameter-be-used-with-application-json-content-type-in-http-1-1).

On the client side you should convert the JSON string to a Javascript object and then extract the message data from this object. The following shows how to do this.

~~~~
var resDataString = httpRequest.responseText;
var resData = JSON.parse(resDataString);
var msg = resData.msg;
// Display msg to the user.
~~~~

Only display the message Hello JSON to the user, do not display the JSON string.

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _JSON assignment_ in the subject line of the email.

