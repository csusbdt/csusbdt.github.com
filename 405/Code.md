---
layout: cse405
---

# Code

## Overview

The purpose of this assignment is to practice applying the source code readability criteria in the syllabus. This will help you get into the habit of following patterns that improve the readability of your code.

## Assignment folder

Create a directory named __code__ in your workspace to hold the work for this assignment. 
When you complete this assignment, the contents of this folder will be the following.

    numbers.js
    colors.js

## Instructions

### numbers.js

The following code is a Nodejs program that prints the first 100 prime numbers. Place this code in a file named numbers.js and verify that it runs as expected.  You can run the program by entering ```node numbers.js``` on the command line.

~~~~
// This program prints the first 100 priime numbers.

// Return true if n is prime.
function isPrime(n) {
  for (var i = 2; i < n; ++i) {
  if (n % i==0) return false;
  }
  console.log(n);
  return true;

}

   var numberOfPrimesDeleted = 0;
var candidatePrime = 2;
var k = 0;

while (numberOfPrimesDeleted<100) {
	if (isPrime(candidatePrime)) {
		++numberOfPrimesDeleted;
	}
	// Increment candidatePrime.
	++candidatePrime;
}
~~~~

The code given above does not satisfy the readability criteria stated in the course syllabus. For this assignment, you should modify numbers.js to resolve these issues. When you are done, the resulting code should produce the same output as the original version and satisfy all of the given readability criteria.

__Go back to the syllabus and study the readability criteria carefully.__

There are more efficient algorithms to accomplish prime number detection than the algorithm used in the code. However, this is not a readability issue, so improving the prime detection algorithm is not required for this assignment. However, you can do this if you want to, just make sure the resulting code satisfies the readability criteria.

### Grading Rubric

I will use the following rubric to compute your grade on numbers.js.

| Criterion                   | Expected Outcome                      | Points |
|-----------------------------|---------------------------------------|:-------:|
| Cleanliness                 | The student removed unnecessary code. | 5 |
| Consistent brackets         | The student maintained a consistent bracketing style. | 5 |
| Logical indentation         | The student fixed the indent that doesn't show a line's inclusion within a parent statement. | 5 |
| Consistent indentation      | The student removed an unnecessary indent. | 5 |
| Logical spacing             | The student adjusted spacing between operators and operands to better convey the order of operations. | 5 |
| Consistent spacing          | The student achieved consistent spacing between operators and operands. | 5 |
| Expressive and clear naming | The student fixed the misnamed identifier. | 5 |
| Clear responsibilities      | The student restructured the code to increase consistency between the name of a function and the function's responsibilities. | 5 |
| Unnecessary comments        | The student removed unnecessary comments. | 5 |
| Spelling                    | The student fixed the spelling error. | 5 |

My rubric template for numbers.js:

~~~~
Cleanliness: 
Consistent brackets: 
Logical indentation: 
Consistent indentation: 
Logical spacing:
Consistent spacing:
Expressive and clear naming:
Clear responsibilities:
Unnecessary comments:
Spelling:
TOTAL:
~~~~

### colors.js

Create a file named colors.js that outputs an HTML document 
that lists 100 random colors.  The generated document should look like
the following except the hexadecimal color values should be random.
Each time you run the program, it should generate 100 different colors values.

~~~
<html>
  <head>
    <meta charset="UTF-8">
    <title>Ten Random Colors</title>
  </head>
  <body>
    <ul>
      <li style="color: #ae3d04">ae3d04</li>
      <li style="color: #ce8cfc">ce8cfc</li>
      <li style="color: #510f40">510f40</li>
      <li style="color: #a256c6">a256c6</li>
      <li style="color: #d85fd1">d85fd1</li>

      ... 100 list items in total

    </ul>
  </body>
</html>
~~~

Each time you run the program it should generate a different
set of color values.  The color values should be generated as 
6 equally likely hexadecimal digits, which means they should 
appear with the same probability.
For the purpose of this assignment, you can assume that the floating point
numbers produced by [Math.random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) 
are equally likely numbers between 0.0 and 1.0.

Send the output to the console by calling console.log.  You can view the HTML
document that you generate by redirecting standard output to a file as follows.

    node colors.js > colors.html

Then, open colors.html in a browser.

Your program must run correctly to get any credit for this part of the assignment.
If it does run correctly, then I will use the following grading rubric.
Each criteria is worth 5 points.
For the organization criteria, using a single file colors.js is good enough.

My rubric template for colors.js:

~~~~
Cleanliness: 
Consistent brackets:
Logical indentation: 
Consistent indentation: 
Portable indentation: 
Logical spacing:
Consistent spacing:
Expressive and clear naming:
Clear responsibilities:
Necessary comments:
Unnecessary comments:
Spelling:
Non-redundant:
Robust to changes:
Organization:
TOTAL:
~~~~

No points will be awarded for submitted programs that do not produce the specified output, so make sure that your 2 programs run as specified in addition to passing the readability criteria. Also, your code should not have any rarely occurring logic errors.

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _Code assignment_ in the subject line of the email.

