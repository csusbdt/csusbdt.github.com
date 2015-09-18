---
layout: cse405
---

# Gems

## Overview

The learning objectives for this assignment include the following.

* Understand how to implement authentication and authorization.
* Understand how to build a single-page Web application.
* Understand how to store application state on the server side.
* Improve your knowledge and skills with the document object model (DOM).
* Understand how to use localStorage to minimize user logins.
* Understand how to implement server-side input validation.
* Understand a simple solution to the problem of stale client-side data.

## Assignment folder

In this assignment, you will extend
[a simple working Web application called gems](https://github.com/csusbdt/gems).
Start by cloning the gems repository into ~/405 and then deleting the .git folder within gems. Here are the commands to use.

~~~~
cd ~/405
git clone https://github.com/csusbdt/gems.git
cd gems
rm -rf .git
~~~~

Deleting the .git folder of the cloned gems project enables you to add the gems project code to your 405 repository.

You should also create a database for use with the project.

    createdb gems

## Instructions

Study the gems project code to understand its function. Then, add a game screen with the following elements.

* a text field that shows how many gems the user has
* a text field that shows the user's score in the game
* a button that when clicked consumes a gem and increases the score by one point
* a button to exit to the main menu screen

The supplied code uses a field named _rev_ to store a revision number for each user row. When you update a row, you need to provide the current revision for the row you are updating. If the revision label you submit does not match the current revision string, then our SQL update operation fails to modify any rows.  This outcome occurs when the application tries to update a row based on stale data (or if another activity deleted the row).

The gem application uses a simple strategy to resolve version conflicts; it replaces any stale information in the user's interface and suggests that the user try again. To see how this works in the starter code for the gems application, open 2 different browsers and log in as user 'a' in both browsers. Buy a gem in one browser window and without refreshing the screen, try to buy a gem in the other browser window. The application updates the balance and gem count that is displayed and then asks the user to try again. As you extend the gems application, you should follow this pattern in each place where you update a document.

The README.md file in the project contains links to videos I made that explain the code.  The videos show a version of the application that is based on CouchDB, which is a document-oriented (no-SQL) database significantly different from PostgreSQL.  Keep this in mind if you watch the videos.
