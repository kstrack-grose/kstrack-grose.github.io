---
layout: post
title:  "Introduction to Promises"
date:   2015-08-20 11:49:23
categories: jekyll update
---

##Introduction
I'm a full-stack engineer, writing primarily in JavaScript. With various queries to databases and servers and APIs, my code is inundated with asynchronous functions. Initially, this meant my code was inundated with nested callbacks, forming messy, tangled chains that were hard to read and hard to follow.

But *then* I discovered promises. Promises are a really great way to deal with asynchronous functions in a more intuitive way. With promises, you can write asynchronous code in the normal, visually pleasing vertical manner, instead of the nested-callback horizontal Triangle of Doom.

##What Are Promises?
As I've said above, promises are a way to deal with asynchronous code. There are many different modules and frameworks around promises, and many database ORMs have their own implementation of promises, but the basic premise remains the same.

**A Promise is an object that promises that it *will* contain a value.** For example, you query your database and the query returns a promise. That promise will be **unfulfilled** while the database is being queried. After the query is finished, the promise will either be **resolved** with the result of the query, or will be **rejected** if the query encountered some error.

##What Do You Do with A Promise?
Once a promise is resolved or rejected, the respective value is passed along to either of these chained functions: **.then** and **.catch**. 

The chained **.then** function will be invoked if the promise (created above it) was resolved. It takes an anonymous function as an argument, and the promise's resolved value will be passed as the only argument to that anonymous function. You can then write out that anonymous function with the series of steps you would like to take with that value--anything you would have done in a callback, you can do here.

The **.catch** function will be invoked if there was an error and the promise was rejected. Like .then, it takes an anonymous function as an argument. The value that will be passed to this anonymous function is the error message that explains *why* the promise was rejected. Once again, you can fill out this anonymous function in the same way you would fill out a callback.

To summarize:

*  You create a promise using an external libarary (see the next section). It will contain the result of an asychronous function.
*  You chain a .then function on to your promise and write an anonymous function as the argument to .then. This will be invoked if the promise is resolved.
*  You chain a .catch function after the .then function and write an anonymous function as the argument to .chain, which will be invoked if the promise is rejected.

In the background, promises really are just like callbacks, with a couple of lovely build-in features. For one, having **.then** or **.catch** automatically invoked according to errors is convenient. Additionally, this chaining method removes some of the awful nesting complexity that is a part of callbacks. Once you wrap your mind around the structure of promises, you'll wonder how you were able to work without them. I promise.

##What Are Some Promise Libraries I Could Use?
The two server-side JavaScript libraries I use to work in promises are [**Q**](https://github.com/kriskowal/q) and [**bluebird**](https://github.com/petkaantonov/bluebird). I also use the [**$q**](https://docs.angularjs.org/api/ng/service/$q) injected dependency when I'm working in angular. There are a few minor differences between libraries, such as the syntax around creating and then resolving or rejecting promises, but the docs with each library are pretty comprehensive.

##Can You Show Me Example Code?
Not at this time, sorry. But I will write a follow-up blog post with example code shortly. In the meantime, I recommend [this video](https://www.youtube.com/watch?v=OU7WuVGSuZw&feature=youtu.be) to go a bit more in-depth with promises.

##Conclusion
I hope this was a good introduction to promises! I've added some resources within the post that will help you continue your promise-related education.