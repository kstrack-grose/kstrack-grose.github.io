---
layout: post
title:  "Creating Bare Servers in Node"
date:   2015-08-21
categories: jekyll update
---
##Introduction
Node.js is a popular framework for using server-side JavaScript. It is most frequently implemented with *express*, a framework that employs middleware to create module and robust applications. As such, most of the documentation and examples on blog posts and StackOverflow are for servers build with express.

But what if you didn't want to use express? Using node without the additional framework makes your code a bit rougher, sure, but the individual steps and paths your code takes is clearer without the smoothing and obfusticating layer of express or another middleware framework.

##Server Structure
Even without the middleware-based express framework, you'll still need a few useful node modules. The basic structure of a bare node server is as follows:

* use the http module to create a server. To keep our code modular, the request handler will be separate.
* set that server to "listen" at an IP address (your local host) and port number.
* create the request handler (below)

##Request Handler Structure
The basic request handler (any request handler) takes in a 
*request* and a *response* as parameters. *request* is the information the client is sending to the server, and *response* is the information you (the server) sends back. 

####Request
The most important information in the request is its method. This will be an HTTP method: GET, POST, PUT, DELETE, or OPTIONS, to name a few. As we'll be implementing only a basic server, we'll just deal with GET and POST.

GET requests are simply asking for information from the server. As the server, you'll need to discover what they're asking for.

POST requests are providing data to the server. You'll need to get this data.

####Response
After you've obtained relevant information from the request, it's time to respond. Responses in bare node servers consist of sending an HTML status code and a header with the type of data the server will respond with and any other relevant information (see the CORS section), writing any other information, and ending the response. Ending the response is crucial; without calling .end() on the response, there's no guarentee the response sent at all.

##Obtaining POSTed Data
Obtaining POSTed data is very easy with express, and slightly more difficult without it. All information over the Internet is sent in *packets*; a bunch of these packets, together, create a single piece of information. Gathering all these packets will look something like this:

{% highlight javascript %}
// create a function to asychronously gather data packets
var getData = function(request, callback) {
  // create a string for easy concatenation
  var data = '';

  // the request will fire a 'data' event every time
  // a new packet is sent
  request.on('data', function(packet) {
    data += packet;
  });

  // an 'end' request will be fired after the final packet
  request.on('end', function() {
    callback(data);
  });
};
{% endhighlight %}

##Example Code
{% highlight javascript %}
// import the http module
var http = require("http");

// our port; there are traditions for what number to use
// but it's mostly arbitrary
var port = 3000;

// set up the IP as your local host
var ip = "127.0.0.1";

// function to handle all the requests to the server
// normally this would be a separate file
var requestHandler = function(request, response) {
  // print the a basic account of every request sent to the server
  console.log("Serving request type "+request.method+" for url "+request.url);

  // every response needs a header; ours is simple
  // and just includes 'content-type'
  // content-type will change depending on the data that
  // your server will respond with.
  var header = {'Content-Type' : 'text/plain'};

  // handle different request methods
  // for these cases, we're only handling requests to the
  // homepage, or '/'. Other URLs would require other if statements
  if (request.method === 'GET' && request.url === '/') {
    // set the status code
    response.writeHead(200, header);
    response.end('GET request successful');
  } else if (request.method === 'POST' && request.url === '/') {
    statusCode = 201;
    // use the getData function defined above
    getData(request, function(data) {
      // asynchronously return this data
      response.writeHead(201, header);
      response.end(data);
    });
  } else if (request.url !== '/') {
     // handle other urls
     response.writeHead(404, header);
     response.end('Not Found');
  } else {
     // handle other request methods
    response.writeHead(505, header);
    response.end('Method Not Supported');
  }
};

// then, create the server
var server = http.createServer(requestHandler);
console.log("Listening on http://" + ip + ":" + port);
// and set it to listening
server.listen(port, ip);
{% endhighlight %}

##How to Test The Code
Basically, you put the code above into a JavaScript file on your computer, run the file with node, and go to http://localhost:3000 You should see 'GET request successful', as per the response in the server!

However, if you don't have node or npm installed, check out my upcoming post about that process. I will also include a section on using the Chrome plugin *Postman* so that you can test the POST method as well.

##CORS and Other Edge Cases
An in-depth discussion of Cross Origin Resource Sharing is beyond the scope of this post, but a quick to-do list would be as follows:

* alter the headers you send in response.writeHead(statusCode, headers) to support cross-origin requests
* add code to your request handler so that it would support an OPTIONS request. 

##Conclusion
I hope this post demystified some of the low-level functionality that happens behind the scenes in express! As always, please email me any corrections/questions/comments/concerns until I get Disqus up on my blog.