---
layout: cse405
---

# Heroku

## Overview

The purpose of this assignment is to learn how to deploy a Web application using the Heroku cloud service platform.

Heroku is a company that provides Web application execution environments as a cloud service. The type of services it provides is referred to as platform as a service (PaaS). Heroku offers a free tier account, which we will use to publish our PostgreSQL-backed gift application.

Create a free Heroku account if you haven't already. Install the Heroku toolbelt on your local system.

After creating an account, login to the heroku system from your terminal window using the following command. You will only need to login once because Heroku will cache your SSH public key to authenticate future connections automatically.

    heroku login

## Assignment Folder

Create folder ~/405/heroku for this assignment. Copy the client, server and scripts folders from the gift assignment into the new heroku folder.

Deployment to Heroku is done by pushing a local Git repository to a remote Git repository. This deployment process requires that certain files be present in the root folder of the repository. For this reason, you will need to create some files in the root folder in addition to creating and editing files in the heroku folder.

Create a Heroku app. Decide on an app name, which must be unique within the Heroku system, and run the following command with it, replacing [app-name] with the app name you choose. Try different names until you find one that has not been taken.

    heroku apps:create [app-name]

The above command might create a new remote reference named heroku that points to the remote repository used for deployment. Check that with the following git command.

    git remote

If you do not see heroku along with master in the resulting list, then use the following to create the remote reference, replacing [app-name] with your chosen app name.

    git remote add heroku git@heroku.com:[app-name].git

Create file ~/405/package.json with the following contents.

~~~~
{
  "name": "[app name]",
  "version": "1.0.0",
  "description": "An app to gift gems",
  "repository": {
    "type": "git",
    "url": "http://github.com/isaacs/npm.git"
  },
  "dependencies": {
    "pg": "3.4.1",
    "st": "0.5.1"
  },
  "engines": {
     "node": "0.10.28",
     "npm": "1.4.9"
  }
}
~~~~

Replace [app name] above with a name that is unique among Heroku apps. The version numbers given above can also be adjusted as needed, but at the time of this writing they work.

Heroku produces a deployable unit called a slug from the files you push. Because you have files that are not needed to produce this slug, you should omit them by adding patterns that match their names to a file named .slugignore.  The syntax for .slugignore is similar to .gitignore. Create file ~/405/.slugignore the omits files not needed to run your application.  For example, my .slugignore file contains the following lines.

~~~~
/git
/hello
/assert
/proto
/scope
/callback
/event
/buffer
/file
/ajax
/json
/curl
/cache
/static
/sql
/read
/gems
/gift
/mobile
/heroku/scripts
~~~~

Use npm to install the Node.js packages specified in package.json.

    cd ~/405
    npm install 

In this assignment, we will run and test the application in both a local development environment and a remote production environment. For this reason, we need add code to distinguish between these 2 environments. We will use Node.js's process object to do this, which was covered in the scope assignment.

Add the following lines to main.js to get the port number from the environment.

    var port = process.env.PORT;
    if (port === undefined) port = 5000;

The above code gets the port number from Heroku's environment. This is the port number the server needs to listen to in the remote environment. When you run in the local environment, you should listen to a port you control, which in our case is port 5000.

Modify the line in main.js that calls the listen function on the server object so that you listen to the port specified by the port variable.

Another area of the code that needs to be modified to adjust to the environment is db.js. In this module, we need to get the database URL from the environment when running in the remote environment. Use the following 2 lines to do this.

    var url = process.env.DATABASE_URL;
    if (url === undefined) url = 'postgres://localhost/gift';

Create file ~/405/Procfile with the following contents.

    web: node heroku/server/main

We are going to issue the node command to start the server from the top level of the repository rather than from within the folder for the assignment. We need to do this because that's how the server will be started in the remote environment. For this to work, we need to adjust the path to app.html that appears in client.js so that it is relative to ~/405.

Add database support to your Heroku app with the following.

    heroku addons:add heroku-postgresql --app [app-name]

Make note of the name of the database that gets created with the above command. In my case, the name was HEROKU_POSTGRESQL_YELLOW_URL.

Promote the database with the following, replacing [db-name] with the name you noted from the previous command.

    heroku pg:promote [db-name] --app [app-name]

The promote command results in Heroku environmental variable DATABASE_URL being set to [db-name]. These environmental variables are set to the database connection string you need to use in your application to connect to the database.

Create file ~/405/heroku/scripts/remote.sh with the following contents, replacing [app-name] with your Heroku app name. (Name the file remote.bat if you are on Windows.)

~~~~
psql -h localhost -d gift -f gift.sql
heroku pg:reset DATABASE_URL --app [app-name] --confirm [app-name]
heroku pg:push gift DATABASE_URL --app [app-name]
~~~~

This script has three commands that does the following three things.

* Recreate a local test database.
* Clear the remote database.
* Copy the local database to the remote database.

Obviously, this script is only used for testing the remote environment and can not be used for a production environment, otherwise you would lose the data in your database.

First test a local deployment. Start by running local.sh to create the local database.

Use the foreman command from the Heroku toolbelt to simulate a launch of the application in the production environment. Try the following, which will fail in some environments.

    cd ~/405
    foreman start

Using the foreman command is better for testing how the app will be launched in the remote environment, but in the past I have observed the foreman command to be problematic in some development environments. If the foreman command fails for you, then try starting the server with the following.

    cd ~/405
    node heroku/server/main

In either case, browse to http://localhost:5000/ and test that the application runs correctly.

After verifying the app runs in the local environment, test the app in the remote environment. Start by running the script to initialize the remote database to a known state.

    ~/405/heroku/scripts/remote.sh

Andrew Yenalavitch made the following comment about this step within a Windows environment:

> For the Heroku assignment, copying the local database via the pg:push command in remote.bat would not work for me in Windows. Whenever I checked the database with pg:info, the tables would always be 0.  Using pg:pull would result in an empty database.  I was able to successfully connect to the database with pg:psql and run the sql commands manually.  Apparently the pg:push command currently has issues in Windows according to the dev:
http://stackoverflow.com/a/26046916


Run the following commands to deploy the application into the remote environment.

~~~~
cd ~/405
git add .
git commit -m "created heroku app"
git push heroku master
~~~~

If there are error messages, then debug. If there are no error messages, then start to view the log file with the following command.

    heroku logs --app [app-name] --tail

Test the app with your browser at the following url.

    http://[app-name].herokuapp.com/

Check the logging output for error messages as you test the app.
