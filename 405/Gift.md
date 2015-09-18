---
layout: cse405
---

# Gift

## Overview

The purpose of this assignment is to develop mastery of all the techniques covered in this course.

This assignment builds on the gems assignment.

## Assignment folder

The starting point for this assignment is the code you developed for the gems assignment. However, don't work in the gems folder; instead, work in a copy of the gems folder named gift. So, start this assignment by making a copy of your gems folder and name it _gift_.

## Instructions

Extend your gems application (the one with the game screen) so that users can gift gems to each other. For simplicity, send only a single gem at a time. Add a gift button to the menu screen that will send the user to an additional screen (the gift screen) that allows the user to gift a gem to another user.

The gifting screen should have the following 3 controls:

* a text field in which the user enters the id of another user
* a SEND button that when clicked sends one gem to the designated user
* a CANCEL button that exits the screen and returns the user back to the home screen

Your code needs to handle the possibility of version conflicts at several points. You should handle these conflicts following the pattern established in the previous assignment: the application updates any stale information in the user's interface and tells the user to try again. However, there is one place that is a little complicated. In the gifting operation, the number of gems in the gift giver document needs to be reduced by one and the number in the gift recipient's document needs to be increased by one. It is possible that one of these operations succeeds and the other fails because one of the rows gets updated by some other activity. For example, imagine the following sequence of events.

* User A submits a request to gift a gem to user B.
* The server reads A's row in the users table.
* The server reads B's row. Suppose the version string for B's document is 100.
* User B submits a request to consume a gem.
* The server reads B's row. The version string is still 100.
* The server continues to handle B's request by writing a new version of B's row showing one less gem. As a result, the database attaches a new version number of 101 to B's row.
* The server continues to handle A's request by trying to write a new version of B's row based on a version number of 100. The update will fail because no row will be found with the B's id and with a version of 100.

In the above scenario, A's request to gift a gem fails when the server tries to update B's document because it has an old version number. Our strategy in this and similar situations is to ask the user to try again. For this to be fair to the user, we should try to add a gem to B's account before subtracting a gem from A's account. If the update to B's account succeeds and then the update to A's account fails, then A is not charged for a gem that gets transferred to B. In a game application, this is probably a better outcome than the other way around, where A is charged a gem that B never gets. For this reason, we should update the gift recipient's document first, and if that succeeds, update the gift giver's document. If we want to guard against this outcome, we would need to either use database transitions or add more complex code that guarantees that the 2 updates occur as a single transaction that succeeds as a whole or fails as a whole. The resulting the application would run more slowly and consume more network and hardware resources. For a game, these additional costs are likely to be unjustified.  Also, if there are a lot of users it is possible that all the data can not be stored on a single database server.  In this case, the data would need to be organized as shards distributed across multiple servers.  Implementing transactions in this context is complex and expensive.

Additionally, make sure you check that the gift recipient is a valid user and that the sender has a gem to send.
