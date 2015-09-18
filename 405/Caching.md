---
layout: cse405
---

# Caching

## Overview

The purpose of this assignment is to learn about HTTP caching mechanisms and how to make use of them in a single-page web application. A good place to start is by reading [Caching Tutorial](http://www.mnot.net/cache_docs/).

Retrieving data from the Internet is slow relative to retrieving from the local file system. Also, retrieving data over the Internet will consume bandwidth, which is a cost for the application provider and user. Because many Web accessible resources, such as HTML, images, video, etc., don't change frequently, it makes economic sense to retrieve these resources once over the network, store them in the local file system, and then read them from the file system when needed subsequently. This process can be called local caching.

There are 2 places where caching of HTTP data can occur:

* on the local computer by the browser, as just described, and
* on a remote computer, called a cache server, that is controlled by your Internet service provider, application provider, or other intermediary interested in conserving bandwidth.

The HTTP protocol includes headers that can be used by client and server to exchange information that is relevant to caching. For instance, some resources should not be cached because they are changing. In this case, we would include cache controlling headers to the request URL to ensure that the resource is never delivered from cache storage. To guard against aggressive caches that might ignore these headers, one can add a random string to the request URL so that the cache sees repeated requests as distinct.

This assignment will illustrate the following 2 techniques.

* How to turn on caching through the use of ETags.
* How to turn off caching.

## Assignment Folder

Create a directory named cache for the work you do for this assignment. At the end of the assignment, this folder will contain the following files.

* app.html
* image.js
* main.js
* message.js
* response.js
* root.js
* image.png

Most of these files can be copied over from the json assignment and modified as required.

## Video

* [Recorded presentation of this assignment](http://youtu.be/SEHmS6B-jeQ)

## Experiment with ETags

Servers can optionally return an ETag header for objects they return to requesting clients. The ETag header contains a string that identifies the version of the object being returned. In this section, you will perform experiments with curl to get a better understanding of how ETags work.

Use curl as follows to view the HTTP response headers when requesting http://curl.haxx.se/.

    curl -X GET http://curl.haxx.se/ -I

These are the headers that I got.

~~~~
HTTP/1.1 200 OK
Date: Wed, 27 Nov 2013 01:54:08 GMT
Server: Apache/2.4.6 (Debian)
Last-Modified: Wed, 27 Nov 2013 01:45:05 GMT
ETag: "2d66-4ec1ebf09370e"
Accept-Ranges: bytes
Content-Length: 11622
Vary: Accept-Encoding
Content-Type: text/html
~~~~

Notice the ETag header. If a browser caches the content returned from a request with an ETag header, then it should also store the ETag value with it. The next time the user navigates to the page, the browser should include an If-None-Match header with the ETag of the cached page to let the server know which version of the page it has cached. In our example, this header would look like the following.

    If-None-Match: "2d66-4ec1ebf09370e"

If the document at the server has not been revised since the last request, then the ETag will not have changed. In this case the server returns a 304 Not modified response. The following command uses the -H option of curl to add an If-None-Match header to the request message.

    curl -X GET http://curl.haxx.se/ -I -H "If-None-Match: \"2d66-4ec1ebf09370e\""

When I ran this command, I got the following response.

~~~~
HTTP/1.1 304 Not Modified
Date: Wed, 27 Nov 2013 01:56:53 GMT
Server: Apache/2.4.6 (Debian)
ETag: "2d66-4ec1ebf09370e"
~~~~

The server responded with a 304 code to let me know that my cached version of the data is current. When you try the above command, make sure that you set the ETag to the one you retrieved from the server.

To see the result of sending a non-matching ETag, try to the following.

    curl -X GET http://curl.haxx.se/ -I -H "If-None-Match: 1234"

When I ran this command, I got the following response.

~~~~
HTTP/1.1 200 OK
Date: Wed, 27 Nov 2013 01:59:23 GMT
Server: Apache/2.4.6 (Debian)
Last-Modified: Wed, 27 Nov 2013 01:45:05 GMT
ETag: "2d66-4ec1ebf09370e"
Accept-Ranges: bytes
Content-Length: 11622
Vary: Accept-Encoding
Content-Type: text/html
~~~~

Notice that the server returns a 200 response code, which means it has included the document in the body of the response. Also observe that the server includes an ETag header that identifies the current version of the document. The server wants me to replace what I currently have stored with the new data and to store the new ETag along with it.

The concept of an ETag is identical to the concept of a version identifier.

## Turn Caching On

This section is focused on controlling caching through ETags. Another common way to control caching is with expiration times, but this is not covered in this assignment.

You should start by making a copy of all the files from the json assignment. This will be the starting point for the remaining experiments in this assignment.

The following code uses the SHA-1 hash function to generate an ETag based on the contents of a buffer. Here, we rely on SHA-1 to generate unique labels for all data to be cached.

~~~~
function generateETag(buffer) {
  var shasum = require('crypto').createHash('sha1');
  shasum.update(buffer, 'binary');
  return shasum.digest('hex');
}
~~~~

You should add the function generateETag to the root module root.js and call it in its init function to generate the ETag for the current contents of app.html.

Generating ETags from file contents is convenient because the ETags will take on new values when files change.

Also, add code that checks the ETag sent by the browser. If the supplied ETag matches the ETag for app.html, then return a 304 Not modified response. If the ETag does not match, then return the new contents of app.html and the new ETag value.

The following function shows how to return a 304 Not modified response.

~~~~
replyNotModified = function(res) {
  res.writeHead(304);
  res.end();
};
~~~~

If in your code etag represents the ETag for app.html, then the following code makes this check and passes control to replyNotModified if the browser's stored ETag matches the current ETag.

~~~~
if (req.headers['if-none-match'] === etag) {
  console.log('returning 304');
  return replyNotModified(res);
}
~~~~

After making these changes to the root module, run the server and test that the caching mechanism works as expected. Use a Web traffic inspection tool to see if your browser checks the ETag for requests it makes after retrieving the root resource. When you do this, make sure that caching is turned on.

Caching by browsers is optional so your browser may choose not to store the object and its ETag. Safari in particular might not respect the ETag you send it. If this is the case with your browser then try a different browser. Alternatively, use curl in the same way that we used it to experiment with http://curl.haxx.se/.

As an additional experiment, change the contents of app.html in some way, then restart the server and verify that the browser retrieves the new version of the Web page. Also verify that subsequent requests for the Web page result in reliance on the local cache.

Modify your application so that ETags are also sent with your image.

Notice that the generateETag and replyNotModified functions are needed in both root.js and image.js. Create a new module named response.js to contain functions that will be accessed from more than one request handling module. Place generateETag and replyNotModified in response.js and remove from root.js and image.js.

## Turn Caching Off

In this part of the assignment, you will ensure that the browser (and any remote caches) do not cache data retrieved via ajax.

Some browsers cache responses to Ajax requests even when using the POST method. See [Is Safari on iOS 6 caching $.ajax results?](http://stackoverflow.com/questions/12506897/is-safari-on-ios-6-caching-ajax-results) For this reason, you should always add a random value to the query string of the URL used with the request message. The request handler on the server should simply ignore this value.

If you set x to the result of calling the Javascript function Math.random in the query string part of the URL, then you will end up with an HTTP request similar to the following. The random component of the URL will ensure that the response is not cached by the client.

    POST /message?x=0.123456789 HTTP/1.1

For the purpose of this assignment assume that the ajax message returned by the server should never be cached. Do the following 3 things to increase the likelihood the response is not cached.

* Add the Pragma: no-cache header to your response.
* Add the Cache-Control: no-cache, no-store header to your response.
* Add a random value to the query string component of the URL sent with the request message.
* Use a Web traffic inspection tool to verify that the browser always retrieves the message.

Adding a random value to the query string of the request URL is probably sufficient to keep the browser from using cached data. However, it's nice to include the headers because they tell browsers to not cache each variation of the request.
