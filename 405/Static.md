---
layout: cse405
---

# Static

## Overview

This purpose of this assignment is to show how to use the Node Package Manager (npm) to install a public module and use the module to simplify your server code.

## Assignment Folder

Create folder ~/405/static to store the files you create for this assignment.  Note that you will also indirectly create folder ~/405/node_modules as a result of running npm as explained below.

The static folder will contain the following files and folder when you have completed this assignment.

* main.js
* message.js
* static (a folder)

The static folder will contain the following files.

* index.html
* image.png
* _any other static resources that you add to your application_

As a starting point, these files can be copied over from the caching assignment.
Rename app.html to index.html.
Do not copy over request.js, response.js, root.js and image.js; you do not need these modules
and so they should be omitted.
Later in the course, we will use modules named request and response
to contain functions that are useful to multiple request handlers;
so you will see these again, but they will not contain different code.

## Instructions

Install [st](https://github.com/isaacs/st) into the root folder of your repository.  Use the following steps from a terminal window.

    cd ~/405
    npm install st

The above command will create a folder named _node\_modules_ to store modules that are retrieved through npm. 
When you run node from within ~/405/static, it will search upward to find the st module stored at ~/405/node_modules/st.

Modify main.js so that requests for static files are handled by st and requests for /message
are still handled by your message module.
The st module is documented in a README.md file in [its source code repository](https://github.com/isaacs/st);
you should refer to this to solve this problem.

Make sure that your solution only relies on st and does not rely on any other module.
In particular, do not use express.

Use the folder named _static_ as described above to hold all files that will be made public.
Rename app.html to index.html and store in the static folder.
Put image.png and any other static files (other images, css, etc.) in the static folder.

In main.js, use the following code to redirect requests for / to /index.html.

    res.writeHead(302, {
        'Location': '/index.html'
    });
    res.end();

Alternatively, you can configure st to return index.html when the root resource is requested 
(requests for '\').  See the st documentation or lecture video to see how this is done.

When you create an instance of st to handle your static content,
make sure that it uses caching and keeps static files resident in memory.

Also, make sure that st serves content from the top level in urls / and not from /static.
This means you should first check if the incoming request is for /message,
in which case you let your message module handle the request.
If the incoming request is for /, then redirect the browser to /index.html.
(See code give above.)
Alternatively, you can configure st to return index.html when the root resource is requested 
(requests for '\').
If the incoming request is not for /message or for /, then let st handle the request.
This means requests for your image will be requested with the path _/image.png_
and not with path _/static/image.png_ even though image.png is stored in the folder named static.

## Lecture Video

* [Recorded presentation on this assignment](http://youtu.be/MNqkEZQ1a5Q)
