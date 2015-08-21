---
layout: post
title:  "Testing Your Server"
date:   2015-08-20 18:35:23
categories: jekyll update
---

##Introduction
In my last blog post, I introduced the concepts behind writing a bare Node server and presented the code do so with. However, if you weren't already familiar with writing servers, you might find it difficult to test the server! This post will take you through all of the steps needed to do so.

##Install Node and npm
[This](https://docs.npmjs.com/getting-started/installing-node) is the documentation I've used to install npm. In short, go to [this](https://nodejs.org/) site to install Node.js. It will automatically install npm. Yay!

Fun fact: npm does **not** stand for anything. 

##Create the Component Files
Next, create a directory for your project. Within that directory, make a JavaScript file with the code from the last blog post. Remember to insert the getData function before the requestHandler function!

The module I used in the last post, http, is included with npm. If you were to use a module that was not included, you'd need to create a package.json file (possibly by following [this documentation](https://docs.npmjs.com/cli/init)) and then type "npm install --save" followed by the module name into the command line. This will install it and save it into your package.json file. 

##Start Server
You're ready to start your server! With your terminal open to the directory where your server file is, simply enter the command "node" followed by the file name. It should run the server and the line "Listening on http://127.0.0.1:3000" will be shown.

##Testing in Browser
Testing in the browser works better when we have a corresponding HTML file, but that's out of the scope of this basic server. However, some basic tests can still be run! Head over to http://127.0.0.1:3000 on a browser. You should see the sentence *'GET request successful'* on the page. If you try any other url, such as http://127.0.0.1:3000/hello, the text *'Not Found'* should be displayed on the page, as per our server routes.

##Testing with Postman
###Intro to Postman
[Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en) is a Chrome plugin that allows you to send customized AJAX requests to a running server. It's a great way to test a server you've written without involving front-end code. Download it and open it up! Near the top, you will see a drop-down menu with different HTTP methods and an input field next to it for the url you'd like to query. Once your server is running, go ahead and try a GET request at http://127.0.0.1:3000. You should see the success message again.

###POST Information
POST requests are slightly more involved. Select POST from the drop-down menu and click on the 'Body' tab. You'll see several options for how to format the data. You want 'x-www-form-unencoded'. Fill out the key and value fields and hit 'Send' again. As per our server, the response should be the data you submitted.

##Conclusion
I hope this was helpful for testing the server we wrote last time! There are a ton of different ways to write servers and to test them, so I hope you continue to play with options and learn.