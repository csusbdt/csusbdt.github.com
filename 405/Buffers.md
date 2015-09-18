---
layout: cse405
---

# Buffers

## Overview

The purpose of this assignment is to learn how to use the Node.js Buffer class to work with chunks of raw data. In this assignment, we create an instance of Buffer to hold the data for a string that is defined in code. In later assignments, the contents of buffers will come from files.

To better understand this assignment, you should read [the Node.js documentation on buffers](http://nodejs.org/api/all.html#all_buffer).

## Assignment folder

Create a directory named _buf_ in your workspace to hold the work for this assignment. When you complete this assignment, the contents of this folder will be the following.

* main.js
* root.js

## Instructions

Create a file named main.js with the following contents.

~~~~
var http = require('http');
var domain = require('domain');
var root = require('./root');

function replyError(res) {
  try {
    res.writeHead(500);
    res.end('Server error.');
  } catch (err) {
    console.error('Error sending response with code 500.');
  }
};

function replyNotFound(res) {
  try {
    res.writeHead(404);
    res.end('not found');
  } catch (err) {
    console.error('Error sending response with code 404.');
  }
}

function handleRequest(req, res) {
  console.log('Handling request for ' + req.url);
  if (req.url === '/') {
    root.handle(req, res);
  } else {
    replyNotFound(res);
  }
}

var server = http.createServer();

server.on('request', function(req, res) {
  var d = domain.create();
  d.on('error', function(err) {
    console.error(req.url, err.message);
    replyError(res);
  });
  d.run(function() { handleRequest(req, res); });
});

server.listen(5000);
~~~~

The above main module code incorporates the domain-based error handling that we covered in a previous assignment. It adds a replyNotFound function, which sends a response to the client when the client requests a resource that the server does not recognize. HTTP responses include 3-digit codes; A code value of 404 signifies that a resource is not found.

The main module also delegates the processing of requests for the root resource path / to a module named root. The next step is to implement the root module.

Create file root.js with the following contents.

~~~~
var http = require('http');

var body = new Buffer("I'm a codfish.", 'utf-8');

exports.handle = function(req, res) {
  res.writeHead(200, {
    'Content-Type': 'text/plain; charset=UTF-8'
  });
  res.end(body);
};
~~~~

In root.js, we create a buffer object to store a string in the UTF-8 character encoding. The UTF-8 encoding is the default encoding, so we could have written the following.

    var body = new Buffer("I'm a codfish.");

However, our code is explicit about the encoding as a form of documentation.

We use the name _body_ to store the string because it will comprise the body of the HTTP message sent to the browser.

When we return the message data to clients, we include a header that indicates the data is of type text/plain. You should read [Internet Media Types](http://en.wikipedia.org/wiki/Internet_media_type) to become familiar with this concept and the standard media type labels such as text/plain, text/html, image/png, application/json, etc.

Following the content type name is a semicolon followed by charset=UTF-8. When sending character data such as text/plain and text/html, the browser needs to know which character set should be used to map byte values to characters. The default character set for strings sent over HTTP is UTF-8, which is a variable length representation of characters. Other character sets are common in other situations. For example, Java programs represent strings internally using the UTF-16 encoding, which is a fixed-length encoding. Javascript uses it own character encoding that is mostly identical to UTF-8 with exceptions for some uncommon characters.

Although UTF-8 is the encoding normally assumed by browsers if not otherwise specified, we include it anyway to ensure that the client interprets the character data correctly.

Test the server code and debug so that it runs correctly.

Node.js uses [chunked encoding](http://en.wikipedia.org/wiki/Chunked_transfer_encoding) by default to send data in the body of HTTP messages. Run your server and verify this with the Web developer tools available for your browser. A good place to learn about how to use the developer tools built into Chrome is [the Dev-tools course on Code School](http://discover-devtools.codeschool.com/). If you are using a different browser, you will need to locate similar documentation and spend time to learn how to use its Web developer tools.

_It is essential to learn Web developer tools for at least one browser._

The alternative to sending data with chunked encoding is to set a Content-Length header. The content length header tells the receiving end exactly how many bytes are included in the body of the HTTP message. Research the HTTP Content-Length header and then modify the root module to send the content type instead of the chunked encoding. I will evaluate your work on this assignment by whether your server code runs correctly with the Content-Length header and satisfies the code readability criteria described in the syllabus.  Setting the Content-Length header significantly boosts performance in Node.js.  (See [High Performance JavaScript with Trevor Norris](http://youtu.be/jsiqvXi3qSA).)

Note: it is possible that the size of the buffer object is larger than the data it holds.
To guard against this possibility, use the following to determine and store the content length.

    var contentLength = Buffer.byteLength(body.toString(), 'utf-8');

## Video

* [Recorded presentation on the Buffers Assignment](http://youtu.be/bYPekNC3Fig)
* Note: After I made the video, I adjusted __replyNotFound__ in these instructions to include exception catching.
* After I made the video, I added the above note on contentLength.  You should use the computed contentLength in your code.

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _Buffers assignment_ in the subject line of the email.

