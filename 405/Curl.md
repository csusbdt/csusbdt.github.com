---
layout: cse405
---

# Curl

## Overview

The purpose of this assignment is to learn how to use the curl command line tool to send and receive HTTP messages, which is useful for system administration, debugging and research.

## Assignment folder

Create a directory named curl for the work you do for this assignment. At the end of the assignment, this folder will contain the following files.

* README.md

## Instructions

Install curl if you don't already have it. If you are on Windows, it is complicated to install curl.  If you want to save time, just use curl on the Linux machines in the lab.

Read about curl and experiment.

View [[http://curl.haxx.se/]] in a browser and then retrieve it using curl.

    curl -X GET http://curl.haxx.se/

Curl writes the contents of the HTTP response into its standard output stream, which you can direct to a file.

    curl -X GET http://curl.haxx.se/ > t.html

Now open t.html in your browser. In Windows, the following will open t.html in your browser.

    start t.html

In OS X, the following will open t.html in your browser.

    open t.html

There's probably a way to do it in Linux, but I don't know what it is, and it may vary based on distribution.

To look at both headers and message body together, do the following.

    curl -X GET http://curl.haxx.se/ -i

To look only at the response headers, do the following.

    curl -X GET http://curl.haxx.se/ -I

In README.md, include the headers that are returned by the previous GET request.
