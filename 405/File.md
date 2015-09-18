---
layout: cse405
---

# File

## Overview

The purpose of this assignment is to learn how to read files into buffers and deliver their contents to requesting clients.

## Assignment folder

Create a directory named _file_ for the work you do for this assignment. At the end of the assignment, this folder will contain the following files.

* app.html
* image.js
* image.png
* main.js
* root.js

## Step 1

This assignment builds on the work you did for the buffers assignment. For this reason, start by creating a folder named file and then copy the contents of the _buf_ folder into it.

Verify that the code you copied into the buf folder runs.

## Step 2

Create a file named app.html and add some simple HTML to it. You will write code that reads the contents of this file when the server starts and returns this data to browsers that request the resource associated with the root path /.

Our strategy will be to read the file app.html into a buffer when the server starts, and return the contents of the buffer for each request sent by browsers the root path /. This procedure is more efficient than reading the file from the file system for each request; however, it requires us to restart the server to test changes to the file.

Replace the contents of file root.js with the following. This code uses the readFile method of the fs module to read the contents of app.html into an instance of Buffer. This is done in a function named init. This function must be called from the main module when the server starts.

~~~~
var http = require('http');

var body;

exports.handle = function(req, res) {
  res.writeHead(200, {
    'Content-Type': 'text/html; charset=UTF-8',
    'Content-Length': body.length
  });
  res.end(body);
};

exports.init = function(cb) {
  require('fs').readFile('app.html', function(err, data) {
    if (err) throw err;
    body = data;
    cb();
  });
}
~~~~

Modify main.js so that it calls the exported init function in the root module before the server starts listening to port 5000. To accomplish this, you need to run the command to listen to port 5000 inside a callback function that you pass into init.

Run and test with browser.

## Step 3

This part of the assignment includes substantially more work than the previous parts and so will require more time to complete.

You need to create a test image in png format for this part of the assignment. Call the image image.png.

Add an img element to your web page that loads an image named image.png from your server. When the browser finds an img tag in an html document, it immediately executes another HTTP GET request for that image.

There are many ways to return the image from the server, but for this assignment you should create a module similar to root.js that does the job. Call the module file image.js. In addition to creating the image module, you will need to modify the main module to call the handle function of the image module when the url of the request object equals /image.png.

When you send your image data to the client, set the Content-Type to image/png and
the Content-Length header with the number of bytes comprising the image data.

Similar to the root module, the image module will have an init function that reads file contents into memory when the server starts.  The init function should take a callback function to call when the file has been
read into memory.

Have the code in main.js call __server.listen(5000)__ only after
both init methods invoke their callback functions.  You'll need to think carefully about how to do this
because it isn't trivial.

Run and test with a browser.

## Video

* [Recorded presentation on the File Assignment](http://youtu.be/HSHULg201YM)

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _File assignment_ in the subject line of the email.

