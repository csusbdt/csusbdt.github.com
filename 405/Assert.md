---
layout: cse405
---

# Assert

## Overview

The purpose of this assignment is to improve your understanding of type coercion in Javascript and the assert module in Nodejs.

[A good source of information about Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) is available through the Mozilla Developer Network.  [Truthy and Falsy: When All is Not Equal in JavaScript](http://www.sitepoint.com/javascript-truthy-falsy/) is a short article the describing some quirky properties in Javascript related to testing for equality.  You should consult with these and other sources of information about Javascript when you do this and other assignments in this course.

Javascript typically runs inside a browser. Inside this runtime environment, programs can access functionality defined as part of the Javascript language and additional functionality provided by the browser. For example, _Error_ is a constructor function that is part of the Javascript language, however _XMLHttpRequest_ is a constructor function that is not part of the language but provided by the application programming interface (API) of browsers.

Similar to browsers, Node.js is an execution environment for programs written in Javascript. However, it offers an API that is different than the one offered by browsers.

The additional functionality of Node.js supports the construction of command line programs in general and servers in particular. Some of this functionality is provided through names in global or module scope. Read [Globals](http://nodejs.org/api/globals.html) for more information.

In Javascript, objects in global scope are attributes of a global object. In browsers, a reference to the global object is obtained through the name _window_; in Node.js, it is accessed through the name _global_.

Outside of global scope, much of the additional functionality provided by Node.js can be accessed through predefined modules. Modules are just Javascript objects, but you might want to think of them as static classes, singleton classes, packages, or namespaces from other languages.

When you write Node.js programs, you will organize your code into modules. The module with a program's entry point is provided on the command line when you run Node.js. For example, suppose you have a file named main.js in the current directory; then you run this as a module with the following command.

    node main

When programs get big, you need to divide the code into separate modules. When one module needs to access the state or functionality of another module, you use a Node.js global function named _require_ to obtain a reference to it. As an example, suppose your program is divided into 2 files main.js and util.js. Inside the main module you can get a reference to the util module as follows.

    var util = require('./util');

This assignment makes use of the Node.js assert module. To use this module, you need to require it as follows.

    var assert = require('assert');

If you pass a true value to assert nothing happens; if you pass a false value, execution terminates with a failed assertion message. For example, the following program will terminate with a failed assertion message.

    assert(2 + 2 === 5);  // generates failed assertion message

The following program will terminate with no message.

    var assert = require('assert');
    assert(2 + 2 === 4);  // no message

## Assignment folder

Create folder <code>assert</code> to hold the results of your work on this assignment. 
When you are finished with this assignment, this folder should contain the following files.

    equality.js
    exception.js

## Instructions

### equality.js

Javascript has 2 equality operators: == and ===. The operator == tries to coerce its operands to the same type and then checks equality. The operator === does not try coercion. For example, the number 0 can be coerced into boolean false. As a result, the following expression evaluates to true.

    0 == false  // true

However, the following is false because === does no type coercion.

    0 === false  // false

The following program shows how to express this using assertion statements.

~~~~
var assert = require('assert');
assert(0 == false);
assert(!(0 === false));
console.log("All tests passed.");
~~~~

We can simplify the last line of the above code with the knowledge that != is the negation of == and !== is the negation of ===.

        assert(0 !== false);

The following table contains pairs of expressions that you should investigate for type-coerced equality (== and !=) and strict equality (=== and !==). In the table LHS is left-hand side and RHS is right-hand side of the equality operator.

| LHS | RHS |
|-----|-----|
| 0 | false |
| 1 | true |
| 3 | true |
| 0 | null |
| 0 | undefined |
| 0 | "" |
| 0 | '0' |
| 0 | 'false' |
| 3 | '3' |
| true | 'true' |
| false | 'false' |
| null | false |
| null | undefined |
| null | 0 |
| undefined | global.x1234 |
| { x: 0 } | { x: 0 } |
| 'a' | 'a' |
| a where var a = 0; | b where var b = 0; |
| c where var c = { x: 0 }; | d where var d = { x: 0 }; |
| c where var c = { x: 0 }; | e where var e = c; |

Create a program in a file named equality.js that contains assertions that demonstrate the type-coercing and non-type-coercing equality between each pair of expressions in the table. Your program would start as follows and continue to include 2 assertions for each row in the table.

~~~~
var assert = require('assert');

// compare 0 and false
assert(0 == false);
assert(0 !== false);

// compare 1 and true
assert(1 == true);
assert(1 !== true);

// compare 3 and true
// ...
// ...
~~~~

Include the following statement as the last line of code in equality.js.

    console.log("All tests passed.");

Your program should therefore display "All tests passed" when it runs.
If it reports an assertion failure, then you need to go back and fix the problem.

### exception.js

The Node.js assert module has another useful assertion function named _throws_, which complains if an assertion is not thrown. The following program runs without any failure messages.

~~~~
var assert = require('assert')

function imbad() {
  throw new Error('I\'m bad.');
}

assert.throws(imbad);

console.log("All tests passed.");
~~~~

Define a function named _divide_ that takes two arguments x and y.  Have the function return x divided by y.  If y equals zero, have the function throw an instance of Error.  Write a program in file exceptions.js that uses the _ok_ and _throws_ functions of the assert module to verify that the function works as expected.  Have the program print "all tests passed" after all assertions are run.

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _Assert assignment_ in the subject line of the email.
