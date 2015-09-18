---
layout: cse405
---

# Course Overview

## Web Applications

Web applications are distributed systems that are minimally comprised of
browser and server components that communicate using the Hypertext Transport Protocol (HTTP).
In this communication, a Web browser initiates a Transmission Control Protocol (TCP) connection
with the server and then uses HTTP to retrieve the client-side application files,
which include HTML, CSS, Javascript, images and possibly other media files.
It uses these files to construct a user interface referred to as a Web page.

Because HTTP is a request-response protocol, 
data at the server must be requested by the client (browser) in order to be presented to the user.
If the server has information that is relevant to the user,
it needs to wait for the client to request it.
This client-server design complicates Web applications when new information on the server
side is created after the Web page has been loaded.
Traditionally, such new data is retrieved by having the client repeatedly send
messages to check for it, which is a practice wasteful of network resources.
To fix this problem, browsers have recently been adding support for _Web Sockets_,
which provide Web applications with access to persistent, two-way TCP connections
that enable server-side components to push data to the client side without waiting for a client
initiated exchange.  However, to be fair, persistent TCP connections also involve some polling between
the two ends to check that the other side is still listening; however, this overhead is
much smaller than the overhead of client-side polling of the server using HTTP.

In this course, we will not look at Web Sockets.
However, it is helpful to keep in mind the basic division between the client-side and
server-side execution environments when you build Web applications.

## Node.js

This course teaches you how to create Web applications with Node.js,
which is a Javascript-based server-side execution environment.

I chose Node.js for the following reasons:

* Students can focus on learning one language for client and server programming: Javascript.
* Learning Javascript on the server side will help students improve their understanding of
  Javascript on the client side.
* It allows for exercises that expose students to relatively low-level details,
  such as HTTP headers, character encodings, and compression.

An important idea to keep in mind when working with Node.js 
is that it is an event-based API that is accessed from a single thread.
This is also true of working with Javascript in the browser.
Event-based APIs complicate the programming of simple applications but
simplify the design of applications with potential concurrency issues.

## How to submit work

In general, each assignment in the course will correspond to a folder in your cse405 Cloud9 workspace.
You submit work sending me an email with a link to your cse405 Cloud9 workspace
to let me know you are submitting the assignment.

I will keep track of your progress in this course with a 
[grade sheet](),
which has a public view that you can check to see your progress.

After you submit work on an assignment for evaluation, I am willing to discuss with you the reasons for the
score you receive, but I will not accept additional work in order to improve your score on the assignment.

## Plagiarism

You should do the coding assignments on your own without copying code from other students or outside sources. If you see a situation where you think it is OK to embed the work of another in your program, then include a note in your source code that identifies the source of the code and send me an email to check whether this is acceptable for the purpose of completing an assignment.

## Submitting Your Work

You do not need to submit anything for this assignment.
