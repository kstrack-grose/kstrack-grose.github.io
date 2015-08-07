---
layout: post
title:  "Numbers in JavaScript"
date:   2015-08-09 13:59:23
categories: jekyll update
---

##Introduction
Okay, here's the deal. JavaScript does not have integers. Other languages have many different data types for numbers (integers, doubles, floats, longs...), but JavaScript just has floats.

##Integers
To expand upon why this distinction is important, I'll start with what, exactly, integers are.

Integers are the numbers that everyone learns in grade school. 1, 2, 3, 4.... All of these numbers are whole numbers, with 0, precisely, after the decimal point. 1 is the same as 1.0 is the same as 1.00000000000000000000000, but this is not the same as 1.000000000000000000000001.

##Floats
Floats are different. The term float, or floating-point-numbers, refers to the fact that in the decimal point *floats* in floating-point-numbers. I'll get into how exactly this works in a moment, but for know, it's enough to know that floats are calculated the same way scientific notation works: you know the digits, and you know what the exponent is (positive or negative), and these relationships are stored rather than the exact number.

##Base 2
To fully understand on how floats affect numerical calcuations in JavaScript, it's important to understand how they are stored in the computer. Each floating point number is stored as three sets of values, in a total of 64 bits:

* 1 bit to store the sign (1 or -1)
* 11 bits to store the exponent, as a power of two
* 52 bits to store the base

The most important piece of information above was the fact that the exponent is not the exponent of a system in base 10, but rather in base 2. The floating point number that is returned from this 64-bit storage is always, always going to be in some form of base 2. Depending on how large or small the base is, the value of calculating the floating-point-number as a value in base 2 numerical system ranges from fairly accurate to not accurate at all.

##Examples
Let's look at how this works in real life. Due to the large number of bits to store the base in, JavaScript remains reasonably (within an integer) accurate below 2^52. At 2^52, or 4503599627370496, JavaScript can only calculate numbers within integer values.

{% highlight javascript %}
console.log(4503599627370496 + 0.4)
// 4503599627370496
console.log(4503599627370496 + 0.6)
// 4503599627370497
{% endhighlight %}

And, if we try to do addition at 2^53, JavaScript can only calculate answers within multiples of 2.

{% highlight javascript %}
console.log(9007199254740992 + 1)
// 9007199254740992
console.log(9007199254740992 + 2)
// 9007199254740994
{% endhighlight %}

##Conclusion
To conclude, JavaScript is bad at math. It's a beautiful and exceptional useful language for many other things, but if you are relying on your programs to provide accurate numerical calculations, you'll want to use another language.