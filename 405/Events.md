---
layout: cse405
---

# Events

## Overview

The purpose of this assignment is to better understand [events in Node.js](http://nodejs.org/api/all.html#all_events).

Events in server-side Node.js are different than events in client-side Web pages, which you also need to learn about, but are not covered in this assignment.

## Assignment folder

Create a directory named event in your workspace to hold the work for this assignment. When you complete this assignment, the contents of this folder will be the following.

* test_instanceof.js
* test_domain.js
* main.js

## EventEmitter

In Node.js, objects that emit events are instances of EventEmitter. Node's HTTP server is an example of an event emitter. You can see this from the following code.

~~~~
var http = require('http');
var EventEmitter = require('events').EventEmitter;
var server = http.createServer();
console.log(server instanceof EventEmitter);
~~~~

Place the above code in a file named test_instanceof.js and run it to verify that it prints true. Modify the code so that it follows the pattern presented in the assert assignment. The program should simply print _All tests passed_ when it runs. Use strict equality === in your assert statements.

The next bit of code shows how to register an event listener function with a server object created by http.createServer().

~~~~
function requestListener(req, res) {
  console.log('Handling request for ' + req.url);
  res.end('hello');
}

server.on('request', requestListener);
~~~~

The first argument in the on function is the name of an event and the second argument is a reference to a function that is called when the event occurs. In our case, the event we listen for is called _request_, which occurs when a browser makes a TCP connection to the port the server is listening to and sends an HTTP request message.

The request event listener function takes 2 arguments: a request object _req_ and a response object _res_. The request object is used to read the HTTP message data sent by the browser to the server and the response object is used to construct an HTTP response message to send from the server to the browser.

There is one more thing we need to do to get a running server: tell the server to listen to a port. Add the following line to do this.

    server.listen(5000);

Assemble the above code snippets into a complete server. Place the code in a file named main.js. You do not need to include the assertion that _server_ is an instance of _EventEmitter_. Run the code and go to [[http://localhost:5000]] with a browser to test.

Your browser may send 2 requests, one for / and the other for /favicon.ico. The favicon file is used by browsers as a graphical icon to represent the site, and it is probably important for a commercial website. However, to keep our examples simple, we will ignore these requests in this course.

Replace your request handling function with the following code.

~~~~
function requestListener(req, res) {
  console.log('Handling request for ' + req.url);
  if (req.url === '/') {
    res.end('hello');
  } else {
    res.writeHead(404);
    res.end('Not found.');
  }
}
~~~~

The above code returns the hello string only for requests for the root resource, which is named by a single slash in the url component of the HTTP request message. For all other requests, the code returns a machine readable status code of 404 and a human readable message in the body of the response message explaining that the resource was _not found_.

Note that the default status code that Node.js will return is 200. That's the reason we didn't specify this when returning the hello string. We could have written the code as follows to be more explicit.

    res.writeHead(200);
    res.end('hello');

## Anonymous functions

In Javascript, functions are objects and are commonly treated as such, especially in the form of callback functions where they are passed in as arguments.

When you read documentation and look at code samples, you will also commonly see unnamed functions used directly in code where they are defined. Because they are defined without a name, they are called anonymous functions.

Anonymous functions are named by the functions that take them as arguments, but these names are local to the called function.

In documentation and articles, you might see the server code presented above written with an anonymous function as follows.

~~~~
server.on('request', function (req, res) {
  console.log('Handling request for ' + req.url);
  if (req.url === '/') {
    res.writeHead(200);
    res.end('hello');
  } else {
    res.writeHead(404);
    res.end('Not found.');
  }
});
~~~~

The definition of the function that was formerly named requestListener is passed directly as an argument to the on function without the need of assigning a name to it.

Modify your code so that your request handler is passed into the _on_ function as an anonymous function.

## Error handling

The material for this section comes from the Domain section of the Node.js documentation. According to this documentation, when Javascript throws an error, the virtual machine may start to leak memory and result in other problems. For this reason, we should avoid writing code that relies on exceptions being thrown as part of its normal operation.

The issue with a Node.js Web server is that an uncaught exception will shut down the process, which shuts off service to all clients as soon as a single uncaught exception occurs. To avoid loss of service, we should catch and report the exception without shutting down the process. The following code demonstrates a way to do this using Domains.

~~~~
var http = require('http');
var domain = require('domain');

function replyError(res) {
  try {
    res.writeHead(500);
    res.end('Server error.');
  } catch (err) {
    console.error('Error sending response with code 500.');
  }
};

function handleRequest(req, res) {
  console.log('Handling request for ' + req.url);
  if (req.url === '/') {
   res.end('hello');
  } else {
    res.writeHead(404);
    res.end('Not found.');
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

Any errors thrown while handling a given request will be caught by a domain object specific to the request in order to avoid halting the server.

Replace main.js with the above version of the server code and test that it runs correctly.

At this point, we have only verified that the server works correctly when there is no error. We will now create a test to see that our error catching code works by artificially throwing an error for every other request for the root resource. Start by making a copy of main.js; call the new file test_domain.js.

In test_domain.js, declare a variable that we will use to toggle between throwing and not throwing an error.

    var throwError = true;

Use the following code as a guide on how to throw an error for every other request for the root resource.

~~~~
if (req.url === '/') {
  throwError = !throwError;
  if (throwError) throw new Error("I'm bad.");
...
~~~~

Run test_domain.js and verify that the server toggles between returning a normal response and an error response. Do this by clicking the browser's refresh button.

## Refactor

In main.js, simplify handleRequest by moving the 404 response code into a function named replyNotFound. Use the implementation of replyError as a model.

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _Events assignment_ in the subject line of the email.

