---
layout: cse405
---

# Scope

##Overview

The purpose of this assignment is to understand scoping in Javascript and Node.js. You will also learn how to access environmental variables in a Node.js program.

## Assignment folder

Create a directory named _scope_ in your workspace to hold the work for this assignment. When you complete this assignment, the contents of this folder will be the following.

* global-scope-1.html
* global-scope-1.js
* global-scope-2.html
* global-scope-2.js
* process.js
* run.sh or run.bat
* mod1.js
* main1.js
* mod2.js
* main2.js
* mod3.js
* main3.js
* function-scope.js
* closure-scope-1.js
* closure-scope-2.js
* closure-scope-3.js

## Global objects

Global references in Javascript are attributes of an object called the global namespace object. In browsers, a reference to global namespace object is window; in Node.js it's global. To see this, create a file global-scope-1.html with the following contents and then load it in a browser.

__global-scope-1.html__

~~~~
<script>
  window.x = 3;
  document.write(x); // 3
</script>
~~~~

<br>
Contrast this with the following Node.js script. Put the following in a file named global-scope-1.js and run it with command `node global-scope-1.js`.

__global-scope-1.js__

    global.x = 3;
    console.log(x); // 3

<br>
In browsers, top-level declarations create references with global scope. Examine the following when loaded in a browser.

__global-scope-2.html__

~~~~
<script>
  var x = 3;
  document.writeln(window.x);  // 3
</script>
~~~~

<br>
However, in Node.js, top-level declarations do not create references in global scope. To place a reference into global scope, you need to explicitly make the reference a property of the global object. To see this, create and run the following.

__global-scope-2.js__

~~~~
var x = 3;
console.log(global.x);  // undefined
global.x = x;
console.log(global.x); // 3
~~~~

## The process object

In Node.js, the process object has global scope. Create and test the following to see this.

__process.js__

    console.log(process !== undefined); // true

<br>
The process object is useful for accessing environmental variables. Add the following to process.js and run to see that x is undefined.

    console.log(process.env.x);

<br>
Now run the code with the environmental variable x set to "Hello there.".

_Under OS X and Linux ..._

    x="Hello there." node process

<br>
_Under Windows ..._

    set x="Hello there." & node process

<br>
The ability to access environmental variables through process.env is useful for running a system with different parameters, such as database connection strings, passwords, URLs, etc. This allows developers to run the system in testing, staging and production environments without modifying the source code.

If you have many variables to set, you can use a shell script to run the program and set each variable on it's own line using the system's line continuation character. As an example, suppose we wanted to run process.js with values set for variables x, y, z.

_Under OS X and Linux ..._

Create a file named run.sh with the following contents.

~~~~
x="Do you like my hat?"   \
y="No I don't."           \
z=42                      \
node process
~~~~

Make the shell script executable.

    chmod +x run.sh

<br>
_Under Windows ..._

Create a file named run.bat with the following contents.

~~~~
set x="Do you like my hat?"
set y="No I don't."
set z=42
node process
~~~~

<br>
Modify process.js to display x, y, and z. Execute your run script to test.

## Module scope

Rather than going into global scope, top-level declarations in Node.js go into module scope, which is a scope added to the language by Node.js. It works similarly to RequireJS and other Javascript libraries that try to provide a module scope that sits between function scope and global scope. Read the [Modules section of the Node.js documentation](http://nodejs.org/api/modules.html) to get a better idea of how Node.js modules work.

References to module instances are obtained by a call to the globally scoped function `require`. Objects in one module can be made accessible to other modules by adding the references to the module's exports property. To see how this works, create files mod1.js and main1.js with the following contents, and then run main1.js.

__mod1.js__

~~~~
var x = 3;
var y = 3;
exports.x = x;
~~~~

__main1.js__

~~~~
var mod1 = require('./mod1');
console.log(mod1.x === 3);
console.log(mod1.y === undefined);
~~~~

<br>
Be careful with the above example. The line `exports.x = x;` adds an attribute named x to the object exports and copies the value 3 into it. The following example shows how this pattern can lead to unexpected results.

__mod2.js__

~~~~
var x = 3;
exports.x = x;
exports.getX = function() {
  return x;
};
~~~~

<br>
__main2.js__

~~~~
var mod2 = require('./mod2');
console.log(mod2.x === 3);
mod2.x = 10;
console.log(mod2.getX() === 3); // true
~~~~
<br>

If we take x out of module scope, we get more intuitive code.

__mod3.js__

~~~~
exports.x = 3;
exports.getX = function() {
  return exports.x;
};
~~~~
<br>

__main3.js__

~~~~
var mod3 = require('./mod3');
console.log(mod3.x === 3);
mod3.x = 10;
console.log(mod3.getX() === 10); // true
~~~~
<br>

Although more intuitive, the code is still not well designed for any purpose I can think of other than illustrating module scope. If you're going to expose x to code outside the module, there's no reason to have a getX function.

## Function scope

Unlike most popular languages, Javascript does not have block scope, which is scoping determined by braces. Instead, it uses function scope and hoisting. Read this [article about scoping and hoisting](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html) and then try to understand what is going on in the following.

__function-scope.js__

~~~~
var x = 1;
var y = 2;

console.log(x === 1);
console.log(y === 2);

function test() {
  x = 10;
  y = 20;
  var y;
}

test();

console.log(x === 10);
console.log(y === 2);
~~~~
<br>

## Closure scope

Closure scope results from Javascript's support for closures. You can think of closures as private spaces that are shared by one or more functions. In the following code, function getX forms a closure with variable x.

__closure-scope-1.js__

~~~~
function getGetXFunction() {
  var x = 3;
  function getX() { return x; }
  return getX;
}

console.log(typeof(x) === 'undefined');

var getX = getGetXFunction();

console.log(getX() === 3);
~~~~
<br>

The code demonstrates that variable x is only visible to getX. The variable x has closure scope in this example.

I wrote the above code to resemble code that you would see in other popular languages. To show you a style that is more common to Javascript programmers, study the following code that illustrates the same closure.

__closure-scope-2.js__

~~~~
var getX = (function() {
  var x = 3;
  return function() { return x; }
})();

console.log(typeof(x) === 'undefined');

console.log(getX() === 3);
~~~~
<br>

In both of the above examples x is hidden from all code except the function getX. The following shows how multiple functions could have access to x.

__closure-scope-3.js__

~~~~
var obj = (function() {
  var x = 3;
  return {
    getX: function() { return x; },
    setX: function(value) { x = value; },
    getNegativeX: function() { return -x; }
  };
})();

console.log(typeof(x) === 'undefined');

console.log(obj.getX() === 3);
console.log(obj.getNegativeX() === -3);
obj.setX(4);
console.log(obj.getX() === 4);
~~~~
<br>

This last example shows how to construct what would be called a singleton in languages that use classes, such as Java, C#, PHP, Python, and C++. A singleton is a class that is intended to be instantiated once in a program.

In Javascript, you can control the visibility of data using closures.

## Rewrite examples

Rewrite the following modules to follow the pattern presented in the assert assignment.

* main1.js
* main2.js
* main3.js
* function-scope.js
* closure-scope-1.js
* closure-scope-2.js
* closure-scope-3.js

These programs should simply print _All tests passed_ when they run. Use strict equality === in your assert statements.

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _Scope assignment_ in the subject line of the email.

