# This article describes how to take an existing Node.js app and deploy it to Heroku.

If you are new to Heroku, you might want to start with the Getting Started with Node.js on Heroku tutorial.

Prerequisites
The best practices in this article assume that you have:

Node.js and npm installed.
an existing Node.js app.
a free Heroku account.
the Heroku CLI.
Overview
The details of Heroku’s Node.js Support are described in the Heroku Node.js Support article.

Heroku Node.js support will only be applied when the application has a package.json file in the root directory.

# Declare app dependencies
The package.json file defines the dependencies that should be installed with your application. To create a package.json file for your app, run the command npm init in the root directory of your app. It will walk you through creating a package.json file. You can skip any of the prompts by leaving them blank.

cd node-example
npm init
...
name: (node-example)
version: (1.0.0)
description: This example is so cool.
entry point: (web.js)
test command:
git repository:
keywords: example heroku
author: jane-doe
license: (ISC) MIT
...
The generated package.json file looks like this:

{
  "name": "node-example",
  "version": "1.0.0",
  "description": "This example is so cool.",
  "main": "web.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "example",
    "heroku"
  ],
  "author": "jane-doe",
  "license": "MIT"
}
To install dependencies, use npm install <pkg> --save. This will install the package and also add it as a dependency in the package.json file. For example, to install express, you would type npm install express --save

Make sure that you are not relying on any system-level packages. Missing dependencies in your package.json file will cause problems when you try to deploy to Heroku. To troubleshoot this issue, on your local command line, type rm -rf node_modules; npm install --production and then to try to run your app locally by typing heroku local web. If a dependency is missing from your package.json file, you should see an error that says which module cannot be found.

# Specify the version of node
The version of Node.js that will be used to run your application on Heroku, should also be defined in your package.json file. You should always specify a Node.js version that matches the runtime you’re developing and testing with. To find your version type node --version.

Your package.json file will look something like this:

"engines": {
    "node": "10.x"
  },
In the Node.js versioning scheme, odd versions are unstable and even versions are stable. The stable branch takes bug fixes only.

Now that the dependencies are installed and the version of node to use has been specified, the package.json file will look something like this:

{
  "name": "node-example",
  "version": "1.0.0",
  "description": "This example is so cool.",
  "main": "web.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "example",
    "heroku"
  ],
  "author": "jane-doe",
  "license": "MIT",
  "dependencies": {
    "express": "^4.9.8"
  },
  "engines": {
    "node": "10.x"
  }
}
It’s a good idea to keep your development environment and production environment as similar as possible. Therefore, make sure that your local version of Node.js matches the version you told Heroku to use in the package.json file. To check which version you’re running locally, at the command line, type node --version.

Specifying a start script
To determine how to start your app, Heroku first looks for a Procfile. If no Procfile exists for a Node.js app, we will attempt to start a default web process via the start script in your package.json.

The command in a web process type must bind to the port number specified in the PORT environment variable. If it does not, the dyno will not start.

For more information, see Best Practices for Node.js Development and Heroku Node.js Support.

Build your app and run it locally
Run the npm install command in your local app directory to install the dependencies that you declared in your package.json file.

npm install
Start your app locally using the heroku local command, which is installed as part of the Heroku CLI.

heroku local web
Your app should now be running on http://localhost:5000/.

How to keep build artifacts out of git
We do not recommend checking node_modules into git because this will cause the build cache to not be used. For more information, see build behavior.

Prevent build artifacts from going into revision control by creating a .gitignore file that looks something like this:

/node_modules
npm-debug.log
.DS_Store
/*.env
Deploy your application to Heroku
After you commit your changes to git, you can deploy your app to Heroku.

git add .
git commit -m "Added a Procfile."
heroku login
Enter your Heroku credentials.
...
heroku create
Creating arcane-lowlands-8408... done, stack is cedar
http://arcane-lowlands-8408.herokuapp.com/ | git@heroku.com:arcane-lowlands-8408.git
Git remote heroku added
git push heroku master
...
-----> Node.js app detected
...
-----> Launching... done
       http://arcane-lowlands-8408.herokuapp.com deployed to Heroku

To open the app in your browser, type heroku open.

# Advanced HTTP features
The HTTP stack available to Common Runtime apps on the herokuapp.com subdomain supports HTTP 1.1, long polling, chunked responses and WebSockets.

# Provision a database
The add-on marketplace has a large number of data stores, such as Postgres, Redis, MongoDB and MySQL.


