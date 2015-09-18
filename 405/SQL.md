---
layout: cse405
---

# SQL

## Overview

The purpose of this assignment is to learn about relational databases and how to use a relational database called [MySQL](http://www.mysql.org).

## Assignment Folder

Create a folder named _sql_ for this assignment.
When you have completed this assignment, you should have a single file in this folder
named _examples.sql_.
Also, when you complete the assignment,
send me a link to your CodeSchool report card.

## Setup

Follow [the instructions on Cloud9 to set up a MySQL database](https://docs.c9.io/docs/setup-a-database) 
in your cse405 worspace.

EDIT FROM THIS POINT DOWN

## Study

Become familiar with the [MySQL documentation](http://dev.mysql.com/doc/).

Complete the [TrySQL tutorial on CodeSchool](https://www.codeschool.com/).

The following are the SQL operations needed to complete the assignments in this course.
Make sure that you cover these operations in your research.

* create a database (this is actually not an SQL command)
* create a table
* delete a table
* insert a new row in a table
* get a row of data using a primary key
* update a row in a table based on a primary key
* delete a row from a table using a primary key

Learn how to perform the above operations from within the mysql command line client.

To practice these operations, you need a database to work on. 
Create a MySQL database named _cse405_ for this purpose.

Create file examples.sql that contains a script that demonstrates the 6 operations listed above.
You should be able to run this script as follows (assuming you created a database named _sam_).

    mysql -u yourusername -p yourpassword cse405 < examples.sql


