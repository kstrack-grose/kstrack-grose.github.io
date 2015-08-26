---
layout: post
title:  "Promises: the Code"
date:   2015-08-26 13:49:23
categories: jekyll update
---

##Introduction
This is the follow-up post for my previous post [about promises](https://kstrack-grose.github.io/jekyll/update/2015/08/20/introduction-to-promises.html). I'll show some code written in a callback structure, the same code written in a promise structure, and discuss why I prefer to use promises.

##Callbacks
Here I've written some code to read the contents of all the files in a directory into a single file in the same directory. You can see the Triangle of Doom--the whitespace created as the callbacks become more and more nested and their closing brackets and parentheses.

{% highlight javascript %}
// require filesystem
var fs = require('fs');

// start by reading the directory
fs.readdir('example/js/', function(err, files) {
  if (err) throw err;
  // create the file we'll write to
  fs.writeFile('example/js/concat', function(err) {
    if (err) throw err;
  });
  // for each file
  for (var i = 0; i < files.length; i++) {
    // open file
    fs.readFile('example/js/' + files[i], 'utf8', function(err, data) {
      if (err) throw err;
      // write a new file with that file component
      fs.appendFile('example/js/concat', '\n\n' + data, function(err) {
        if (err) throw err;
        console.log('successfully appended text to the file!');
      });
    });
  }
});

{% endhighlight %} 

If one is familiar with callbacks, this is simple sequence to read. However, as code becomes more complicated and especially as it spans multiple files, callbacks become more and more difficult to follow. Enter promises!

##Promises with Q
I've written the code below using the [**Q**](https://github.com/kriskowal/q) library. If you're unsure how to use npm modules, check out my post on [testing Node files](https://kstrack-grose.github.io/jekyll/update/2015/08/20/testing-your-server.html).

{% highlight javascript %}
// require filesystem and Q
var fs = require('fs');
var Q = require('Q');

var readDir = Q.nbind(fs.readdir);
var newFile = Q.nbind(fs.writeFile);
var readFile = Q.nbind(fs.readFile);
var addToFile = Q.nbind(fs.appendFile);

readDir('example/js/')
.then(function(allFiles) {
  // make the catchall file
  newFile('example/js/concat')
  // don't need to do anything when it works, just catch
  .catch(function(err) {
    console.log(new Error(err));
  });
  // for each file
  for (var i = 0; i < allFiles.length; i++) {
    readFile('example/js/' + allFiles[i], 'utf8')
    .then(function(fileData) {
      addToFile('example/js/concat', '\n\n' + fileData)
      .then(function() {
        console.log('successfully appended text using Q!');
      }) // end of add to file
      .catch(function(err) {
        console.log(new Error(err));
      });
    }) // end of readfile
    .catch(function(err) {
      console.log(new Error(err));
    }); 
  }; // end of for loop
}) // end of readDir 'then'
.catch(function(err) {
  console.log(new Error(err));
}); // end of readDir
{% endhighlight %} 

You can see that the code written in promises is longer, but you can also intuit how separating the successes (**.then**s) and failures (**.catch**es) become very useful. As I alluded above, I find promises especially rewarding when I need to use an asychronous value across multiple files, such as making an API request.

##Conclusion
I hope the code I've been able to show you has illuminated some of the concepts I brought up in my previous post. As always, contact me with any questions/comments/corrections.