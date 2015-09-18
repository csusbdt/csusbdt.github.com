---
layout: cse405
---

# Read

## Overview

The purpose of this assignment is to learn how to read data from a PostgreSQL database from inside a Node.js program. The procedure for accessing other SQL databases is similar.

This assignment builds on the ajax and sql assignments. In the ajax assignment, we created a Web page that uses ajax to request a message string from the server, which it displays to the user. However, the message string was hard-coded into the program. In this assignment, we will use what we learned in the sql assignment to retrieve the message string from a PostgreSQL database.

## Assignment Folder

Create folder ~/405/read to contain the work you do on this assignment.

Copy your app server code from the static assignment to the read folder as a starting point.

Test that the application is working correctly by visiting http://localhost:5000/ in a browser.

## Install Database Driver

We will use a Node.js module named _pg_ to issue commands to the database and receiving their results.
Do the following to install the pg module.

    cd ~/405
    npm install pg

The above command stores the module inside the folder ~/405/node_modules that already exists in your repository.

The above command sometimes fails under Windows, displaying an error message with a path to a directory that ends with _AppData\npm_.  To fix the problem, run a terminal window as administrator and create the npm folder under AppData.  Then try the npm install command again.

## Step One: Create Stubs

In this step we create a partially implemented module named db that provides a layer of abstraction between the request handlers and the database. The db module exports a single function named getMessage. We also modify message.js, the module that handles requests for the message. Actual interaction with the database will be done in a later step.

Create ~/405/read/db.js with the following contents.  The purpose of the `id` is to identify a given string in a table of string values.  Note that we return `null` when the id is not found in the database table; this is a common convention.

~~~~
exports.getString = function(id, cb) {
  if (id === 'message') cb(null, Math.random() + '');
  else cb(null, null);  // Return null when id is not a key in the string table.
};
~~~~

The getString function is implemented as a stub for this intermediate step of development. The purpose of this function is to retrieve a message string from the database. If for whatever reason there is a problem getting this string, the callback function cb is called with an instance of Error as its sole argument. If there is no problem, then cb is called with null as its first argument and the message string as its second argument.

We use the random number generator to simulate changing message data. The random function returns a number, which we convert to a string by concatenating with the empty string.

There is one issue with the stub code for the getMessage function: it invokes the callback immediately. When interacting with the database, we will need to invoke asynchronous functions, which means we will return from getMessage before the callback cb is called. But our stub code returns from getMessage after the callback cb is called. As a result, the stub code might permit logic errors that could be caught at this point. The following code is an improved version of the stub code.

~~~~
exports.getString = function(id, cb) {
  setTimeout(function() {
    if (id === 'message') cb(null, Math.random() + '');
    else cb(null, null);
  }, 4);
};
~~~~

The above version of getString has the function returning before the callback is invoked, which is the normal sequence of events with asynchronous functions.

Another improvement we can make is to alternate between returning an error and returning a message. This will test our error handling code. The following version of the stub code accomplishes this.

~~~~
var error = false;

exports.getString = function(id, cb) {
  setTimeout(function() {
    if (error) cb(new Error('database server error'));
    else if (id === 'message') cb(null, Math.random() + '');
    else cb(null, null);
    error = !error;
  }, 4);
};
~~~~

With our database code stubbed out, we can now make changes to the message request handling code, which is in the message module. The following code is a rewrite of this module so that the getString function of the db module is used to get the message to return to the client. 

~~~~
var db = require('./db');

exports.handle = function(req, res) {
  db.getString('message', function(err, message) {
    if (err) {
      console.log(err.message);
      res.writeHead(500, 'server error');
      res.end();
      return;
    }
    var responseObject = { msg: message };
    var responseString = JSON.stringify(responseObject);
    var responseBody = new Buffer(responseString, 'utf-8');
    res.writeHead(200, {
      'Content-Type': 'application/json',
      'Content-Length': responseBody.length,
      'Pragma': 'no-cache',
      'Cache-Control': 'no-cache, no-store'
    });
    res.end(responseBody);
  });
};
~~~~

Note that we require the db module using an absolute pathname './db'. We do this because Node.js does not search the current directory when resolving references to modules.

Also note that we need to create a Buffer object for each request. This is required because the message data may change between requests.

Test your implementation by running the server and testing with a browser.

## Step Two: Write Test Database Creation Script

Start your PostgreSQL server if not already started.

Create a database called _read_.

    createdb read

Add commands to a file named _init.sql_ to create a table called _strings_ with two columns: a primary key column named _id_ of type varchar(255) and a column named _value_ of type _text_.  Add a command to insert a single row into this table with _id_ set to "message" and _value_ set to "the read assignment".

## Step Three: Complete the Database Module

Replace the stub code in db.js with the following code.  On Windows, you may need to try different connection string, such as 'postgres://postgres@localhost/read' or 'postgres://username:password@localhost/read'.

~~~~
var pg = require('pg');

var url = 'postgres://localhost/read'; 
// If on Windows, use 'postgres://postgres@localhost/read' or 'postgres://username:password@localhost/read' 

exports.getString = function(id, cb) {
  pg.connect(url, function(err, client, done) {
    if (err) return cb(err);
    client.query('select value from strings where id = $1', [id], function (err, result) {
      done();
      if (err) return cb(err);
      if (result.rows.length === 0) {
        return cb(null, null);
      }
      cb(null, result.rows[0].value);
    });
  });
};
~~~~

Test that the browser gets the current message from our server and the server code runs without error.

If you have trouble connecting to the database, try creating a user named postgres as follows:

    createuser postgres --superuser
