---
layout: cse405
---

# Prototypes

## Overview

The purpose of this assignment is to learn about Javascript's prototypal inheritance mechanism. The focus will be on using inheritance in client-side code that interacts with the document object model (DOM).

## Assignment folder

Create folder _proto_ to hold the results of your work on this assignment. When you are finished with this assignment, this folder should contain the following file.

    app.html

## Reading

Read [Prototypal inheritance](http://javascript.info/tutorial/inheritance) and [Understanding JavaScript Prototypes](http://javascriptweblog.wordpress.com/2010/06/07/understanding-javascript-prototypes/).

## Example

Study the following code.

~~~~
<html>

<head>
  <meta charset="UTF-8">
  <title>Screen Constructor</title>
  <style>
    #login-screen, #menu-screen {
      display: none
    }
  </style>
</head>

<body>

<div id="login-screen">
  Username: <input id="login-username-box" type="text" size="20" /> <br>
  Password: <input id="login-password-box" type="password" size="20" /> <br>
  <button id="login-submit-btn">Login</button>
</div>

<div id="menu-screen">
  <p>This is the menu screen. 
     Normally, there would be a menu of choices.
     But this is a stub, so you can only logout from here.
  </p>
  <button id="menu-logout-btn">Logout</button>
</div>

<script>
// Create one global object to hold all application data and logic.
var app = { 
  screens: {},   // application screens
  Screen: null   // screen constructor
};

// Start the definition of the Screen constructor.
app.Screen = function(name) {
  this.name = name;
};

// Define a "static" variable to refer to the currently active screen.
app.Screen.currentScreen = null;

// Define a function that Screen instances inherit.
app.Screen.prototype.getDiv = function() {
  return document.getElementById(this.name + '-screen');
};

// Define another function that Screen instances inherit.
app.Screen.prototype.show = function() {
  if (app.Screen.currentScreen) app.Screen.currentScreen.getDiv().style.display = 'none';
  this.getDiv().style.display = 'block';
  app.Screen.currentScreen = this;
};

// At this point, definition of the Screen constructor is done.

// Create the screen instances.
app.screens.login = new app.Screen('login');
app.screens.menu = new app.Screen('menu');

// Set a click event handler for the login button in the login screen.
document.getElementById('login-submit-btn').onclick = function() {
  app.screens.menu.show();
}

// Set a click event handler for the logout button in the menu screen.
document.getElementById('menu-logout-btn').onclick = function() {
  app.screens.login.show();
};

// Start in the login screen.
app.screens.login.show();
</script>

</body>
</html>
~~~~

Place the above code in a file named app.html. Load the file in your browser and observe its behavior.

## Add a Game Play Screen

Define an additional screen in app.html named gameplay.

Add a "Play Game" button to the menu screen. 
When the user clicks the play game button, transition to the game play screen. 

Include an "Exit" button in the gameplay screen. When the user clicks the exit button, 
transition to the menu screen.

## Add an Hide Method

The example Screen constructor has two methods that instances inherit: getDiv and show. 
Add a third method named hide that when called sets the display attribute of the screen div to none.
Make sure that your code makes sense from a readability perspective and is not
written to simply work.

Use the new hide method to simplify the show method as follows.

~~~~
app.Screen.prototype.show = function() {
  if (app.Screen.currentScreen) app.Screen.currentScreen.hide();  // revised code
  this.getDiv().style.display = 'block';
  app.Screen.currentScreen = this;
};
~~~~

## Submitting Your Work

Send me an email with a link to your workspace.  
Write _Prototype assignment_ in the subject line of the email.

