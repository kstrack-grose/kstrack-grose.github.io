---
layout: post
title:  "Testing Promises with Chai"
date:   2015-09-01 22:09:23
categories: jekyll update
---

##Introduction
I've already written [a few](https://kstrack-grose.github.io/jekyll/update/2015/08/20/introduction-to-promises.html) [posts](https://kstrack-grose.github.io/jekyll/update/2015/08/26/promises-the-code.html) about my favorite method for working with asynchronous code: promises. This week I created a lot of tests for asynchronous code (HTTP requests and database queries, primarily) and I'm excited to share more promise-related information with you all.

##The Modules
This post assumes some knowledge of testing frameworks, and (as usual) is intended for code in JavaScript. These tests I've been writing have been built with the [Mocha](https://mochajs.org/) framework, the [Chai](http://chaijs.com/) assertion library, and the Chai add-on [Chai as Promised](https://www.npmjs.com/package/chai-as-promised).

####Mocha
Once mocha is installed via npm, you can test in the command line. To do so, go into the directory where your test file is and run `$ mocha [test file name]`. If your test file is named *test.js*, you only have to run `$ mocha`. However, if your test file named *databaseTesting.js*, you'll have to run `$ mocha databaseTesting.js`.

####Chai
These are well documented, but just to condense some information for you, I've included the code to set up all of these libraries properly.

{% highlight javascript %}
var chai = require("chai");
var chaiAsPromised = require("chai-as-promised");
 
chai.use(chaiAsPromised);

var expect = require('chai').expect;
var should = require('chai').should;
{% endhighlight %}

Then, to set up your tests, write up the framework code:

{% highlight javascript %}
// describe the concept you'll be testing
describe('Arrays', function () {
  // set up any variables that all your tests should have access to
  var arr = [1, 2, 3];

  // write a descriptive test expectation
  it('should return -1 when the value is not present', function () {
    // and then use the assertion library to write the test
    expect(arr.indexOf(4)).to.equal(-1);
  });
});
{% endhighlight %}

I'm not going to go into detail on the assertion library--as I've said, it's well documented and the information would be too much for the scope of this post.

####Chai As Promised
The most important and most obvious part of the chai-as-promised module is that it has the chainable **.eventually** property. When this is used in conjunction with the rest of the Chai assertion library, it indicates that Chai should wait for a promise to be fulfilled before it evaluates the test.

##Testing With Promises
The example test above is pretty intuitive. The parameter to **expect()** is evaluated and compared to the parameter to **.equal()**. Well, the same concept will be applied to asynchronous promises.

Intuitively, I would put the testing assertion in the final chained .then() function. Since promised code forces asychronous code to be sychronous, it would seem to make sense that way--at the end of the chain of synchronous functions, evalue what you want to evaluate, right?

Wrong. It turns out that this doesn't work, and here's why.

Let's return to a principle from my first post about promises and use that information in conjunction with how we know *expect()* works. *A promise is evaluated to whatever returns from the final chained or nested* **.then()**. Now, if we consider that in conjunction with what I mentioned at the beginning of this section (*the parameter to expect() is evaluated and compared to the parameter to .equal()*), the correct syntax for chai-as-promised becomse clear.

The entire promise, along with any chained .then()s or .finally()s must be passed to .expect(). .expect() is then chained with .eventually, and the test will evaulate the result of the *entire* promise when it is returned.

Side note: avoid chaining *.catch()*s inside of *expect()*. This is because you want chai-as-promised to catch errors that occur, so that the tests are accurate. If you catch them before the promise is evaluated, the test will pass and thus not give you a clear indication of when things are going wrong.

##The Code
That was a lot of words, so let's look at some code. I'm pulling code from the test suite that I just wrote, so the examples may seem really specific. The example below tests the insertion of a new user into a relational database using [Bookshelf.js](http://bookshelfjs.org/).

{% highlight javascript %}
describe('User Model and Users Collection', function() {
  // set up the variables you want to be available for all tests
  // in this 'describe' function
  var testEmail = 'test@email.com';
  var testPassword = '1234';

  it('should add a user model', function() {
  // pass the saved new user (a promise) into expect
    expect(new User({email: testEmail, password: testPassword}).save())
    // then finish the assertion, using 'eventually'
    .to.eventually.be.fulfilled;
  });
 }); 
{% endhighlight %}

Make sense? Since that was a pretty simple example, I'm going to include another, more involved example. Assume that this test is within the same 'describe' function.

{% highlight javascript %}
  it('should remove a user model', function() {
    // note that every single chained promise and method is within .expect()
    expect(new User({email: testEmail})
      .fetch()
      .then(function(user) {
        var id = user.get('id');
        return new User({'id': id})
        .destroy()
        .then(function(user) {
          return id;
        })
      })
      .then(function(id) {
        return new User({email:testEmail}).fetch()
      }))
    // and that we evaluate the entire chain as one
    .to.eventually.be.null;
  }); 
{% endhighlight %}

##Conclusion
Don't worry if you didn't follow all of the Bookshelf.js methods in the last example: the point was simply that the *entire* promise must go inside the expect() assertion when you use chai-as-promised.

Now you can go forth and test the promises you wrote after the last couple of blog posts! Enjoy.





