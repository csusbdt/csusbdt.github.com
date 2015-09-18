---
layout: cse405
---

# Callbacks

## Overview

At any given moment, a server program may be required to handle requests from several connecting clients simultaneously. Handling these requests typically requires access to resources through relatively slow file and socket input/output (I/O) streams. We say that server processes are __I/O bound__ because most of the time is spent waiting on I/O operations to complete.

Typically the runtime environment provides I/O operations that block, which means that execution waits until the operation completes before processing continues. Blocking I/O is simpler to understand because the operation completes before control returns from the function. The following is pseudo code that shows how to get a byte from a file input stream.

~~~~
byte b = fileInputStream.readNextByte()
// do something with b
~~~~

Non-blocking I/O involves passing in a callback function to the operation. The following shows in pseudo code how to get a byte of data from a file input stream.

~~~~
define function f(byte b) {
  // do something with b
}
    
fileInputStream.readNextByte(f)
~~~~

Mainstream environments such as PHP, ASP.NET, and Java Servlets use blocking I/O. In these environments, request handlers run in separate threads to handle requests in parallel. If the threads need to access common data, then synchronization techniques are needed.

Node.js provides a single-threaded environment with a non-blocking API. This means that thread synchronization is not needed but callback functions need to be passed into API calls that perform I/O or other long-running activities.

The purpose of this assignment is to better understand how callbacks are used in Javascript.

## Assignment folder

Create folder _callback_ to hold the results of your work on this assignment. 
When you are finished with this assignment, this folder should contain the following files.

* geturl.js
* error.js
* util.js
* main.js

## Reading

The following code shows how to retrieve data from a server using the HTTP communications protocol. The code illustrates callbacks.

~~~~
var http = require('http');

function onData(chunk) {
        console.log("Got a chunk of data of size " + chunk.length);
}

function onEnd() {
        console.log("Response message received in full.");
}

function onResponse(res) {
        console.log("Response status code: " + res.statusCode);
        res.on('data', onData);
        res.on('end' , onEnd);
}

function onError(e) {
        console.log("Got error: " + e.message);
}

var req = http.get("http://www.google.com/index.html", onResponse);
req.on('error', onError);
~~~~

Study the following documentation to understand the above code.

* http://nodejs.org/api/http.html#http_http_request_options_callback
* http://nodejs.org/api/http.html#http_event_response
* http://nodejs.org/api/http.html#http_http_incomingmessage
* http://nodejs.org/api/stream.html#stream_class_stream_readable

## Video

You can optionally watch a [video presentation of this assignment](http://youtu.be/8K-aOkQcJ-s).

## Anonymous Functions

Javascript allows the use of anonymous functions, which allows you to use a function definition in place of a reference to a function.  The following code shows how to rewrite the program in the previous section so that an anonymous function is used rather than the named function _onResponse_.

~~~~
var http = require('http');

function onData(chunk) {
        console.log("Got a chunk of data of size " + chunk.length);
}

function onEnd() {
        console.log("Response message received in full.");
}

function onError(e) {
        console.log("Got error: " + e.message);
}

var req = http.get("http://www.google.com/index.html", function(res) {
        console.log("Response status code: " + res.statusCode);
        res.on('data', onData);
        res.on('end' , onEnd);
});

req.on('error', onError);
~~~~

As an exercise, refactor the above code so that the functions onData, onEnd and onError are replaced by their anonymous equivalents.  The resulting program should produce the same result.  Place your program in a file named _geturl.js_.  Test your program and debug as needed to get it to run correctly.

## Callbacks That Take Errors

A convention that many Node.js programmers follow is as follows: if a function that takes a callback can generate an error, then the first argument of the callback holds the error. This first argument, typically named err, is either set to null (in the case of no error) or set to an instance of Error (in the case of an error). If the function needs to return data to calling code, then it passes this as the second argument in the callback function. If there is no error, then the function calls the callback as follows.

    cb(null, data);

If there is no data to return, then the function calls the callback as follows.

    cb(null);

If there is an error, then the function calls the callback as follows.

    cb(err);

The first thing the calling code should do is check whether err is null.

~~~~
if (err) {
    // Handle error.
} else {
    // Handle normal case.
}
~~~~

Create file error.js to hold a program that illustrates these error handling conventions.  In error.js, define a function named divideby that illustrates the conventions just described. The following is a description of the divideby function that you should implement.

~~~~
function divideby(x, y, cb) {
    // If y is zero, return an instance or Error in the first argument of cb.
    // Otherwise, divide x by y and return the result in the second argument of cb
    // and set the first argument to null to indicate no error.
}
~~~~

You also need test code that calls divideby.  Complete the following code and
include in your program.

~~~~
divideby(6, 3, function(err, result) {
    // Assert that err is null.
    // Assert that result is 2.
});

divideby(6, 0, function(err, result) {
    // Assert that err is not null.
    // Assert that result is undefined.
    // Assert that type of err.message is a string.
});
~~~~

The function _Error_ is a constructor built into Javascript. To create an instance of Error, pass a string to it as follows.

    new Error('Division by zero is undefined.');

## Module

Create a module named _util.js_ that exports a function named _geturl_
that implements the logic in the geturl function you placed in geturl.js 
from an earlier point in this assignment.
Create another file named _main.js_ that contains the following test code for the
geturl function in your util module.

~~~~
var util = require('./util');

util.geturl('http://a.bad.domainname/index.html', function(err, content) {
        if (err) {
                console.log(err);
        } else {
                console.log(content);
        }
});

util.geturl('http://www.google.com/index.html', function(err, content) {
        if (err) {
                console.log(err);
        } else {
                console.log(content);
        }
});
~~~~

Notice that the callback the geturl takes follows the error convention described above.
Also notice that the callback takes as its second argument the content that was
retrieved from the url.  Inside util.geturl, you will need to concatenate the chunks of data
as they arrive and pass the accumulated data to the callback after the entire response is received.

Rework the util module so it works correctly with the
test code provided above.  Do not change the test code provided.

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _Callbacks assignment_ in the subject line of the email.

