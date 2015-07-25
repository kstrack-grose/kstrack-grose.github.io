---
layout: post
title:  "Follow-Up to Conflict Resolution Post"
date:   2015-07-16
categories: jekyll update
---

##Introduction
Hi, everyone. As the title implies, this is a follow-up post to the recent one I wrote about alternate conflict resolution in hash tables. I've gotten a few questions about the particulars of my implementation, so I decided to post the code that I've written.

##Pseudocode
{% highlight javascript %}
// create your hash table--it takes in a length
  // assign that length to a property
  // and create a storage array
  // also create a total values property

// create an insert function with parameters key and value
  // find the index by hashing the key using the first hash function
  // while that index is occuped and not -1
    // if the first value in that index is the key you're passing in
      // change the value in that index
      // and you're done with insert
    // if the first value is not the key you're passing in
      // change the index by adding the key, hashed by the second hash
      // function, and then using modulo to ensure that the index is
      // always within the hash table's limit and continue with the
      // while loop until you reach an empty index in the hash table
  // you're at an empty index in the hash table, so insert the
  // key/value pair as a tuple in this index
  // increment total values
  // and check size

// create a retrieval function with a parameter of key
  // hash the key using the first hash function to find the first
  // possible index
  // while the key you're trying to retrieve is: -1 or a different key
    // find another index by adding the value returned by the second
    // hash function to the current index & ensure it's within bounds
  // if we're at a filled index where the key is key
    // return the value
  // else, we're at an undefined index and that key has never been
  // inserted in the first place. Return null.

// create a removal function with a parameter of key
  // hash the key using the first hash function and look at that index
  // while the key at that index is -1 or a different key,
    // find another index by adding the value returned by the second
    // hash function to the current index & ensure it's within bounds
  // if we're att a filled index where key is key
    // overwrite the index as -1
    // decrement total values
    // and check size
  // else, the key you're trying to remove doesn't exist. Return null.

/* checkSize helper function
 * this would be a checkSize method of our HashTable constructor. It 
 * would compare the total values with the hash table limit, and if
 * total values < 25% of limit, it would implement the reduce size 
 * helpers function (below).
 * If total values > 75% of limit, it would implement the increase
 * size helper function (also below).
*/

/* reduce size helper function
 * this function would create a copy of the hash table's storage
 * and then change the limit to be half of the previous limit, and
 * reset the number of values to 0. It would then insert all of
 * the values in the old storage property, creating a new hash table
 * with half the size and all of the properties.
*/

/* increase size helper function
 * this function would go through the same process as reduce size,
 * but it would double the size property.
*/
{% endhighlight %}

##Functional Code
The title of this section is a bit of a misnomer; without implementing the helper functions I outlined above (checksize, reduce size, increase size, and two different hash functions), you won't be able to implement the hash table. However, the primary functionality is below.

{% highlight javascript %}
// create your hash table--it takes in a length
var HashTable = function(length) {
  // assign that length to a property
  this.limit = length;
  // and create a storage array
  this.storage = [];
  // also create a total values property
  this.numOfValues = 0;
};

// create an insert function with parameters key and value
HashTable.prototype.insert = function(key, value) {
  // find the index by hashing the key using the first hash function
  var index = hashOne(key, this.limit);
  // while that index is occuped and not -1
  while (this.storage[index] && this.storage[index] !== -1) {
    // if the first value in that index is the key you're passing in
    if (this.storage[index][0] === key) {
      // change the value in that index
      this.storage[index][1] = value;
      // and you're done with insert
      return;
    } 
    // if the first value is not the key you're passing in
    else {
      // change the index by adding the key, hashed by the second hash
      // function, and then using modulo to ensure that the index is
      // always within the hash table's limit and continue with the
      // while loop until you reach an empty index in the hash table
      index = (index + hashTwo(key, this.limit)) % this.limit;
    }
  }
  // you're at an empty index in the hash table, so insert the
  // key/value pair as a tuple in this index
  this.storage[index] = [key, value];
  // increment total values
  this.numOfValues++;
  // and check size
  this.checkSize();
};


// create a retrieval function with a parameter of key
HashTable.prototype.retrieve = function(key) {
  // hash the key using the first hash function to find the first
  // possible index
  var index = hashOne(key, this.limit);
  // while the key you're trying to retrieve is: -1 or a different key
  while (this.storage[index] === -1 || (this.storage[index][0] && this.storage[index][0] !== key)) { 
    // find another index by adding the value returned by the second
    // hash function to the current index & ensure it's within bounds
    index = (index + hashTwo(key, this.limit)) % this.limit;
  }
  // if we're at a filled index where the key is key
  if (this.storage[index][0] === key) {
    // return the value
    return this.storage[index][1]; 
  } else {
    // else, we're at an undefined index and that key has never been
    // inserted in the first place. Return null.
    return null;
  }
};

// create a removal function with a parameter of key
HashTable.prototype.remove = function(key) {
  // hash the key using the first hash function and look at that index
  var index = hashOne(key, this.limit);
  // while the key at that index is -1 or a different key,
  while (this.storage[index] !== -1 && this.storage[index][0] !== key) {
    // find another index by adding the value returned by the second
    // hash function to the current index & ensure it's within bounds
    index = (index + hashTwo(key, this.limit)) % this.limit;
  }
  // if we're att a filled index where key is key
  if (this.storage[index][0] === key) {
    // overwrite the index as -1
    this.storage[index] = -1;
    // decrement total values
    this.numOfValues--;
    // and check size
    this.checkSize();
  }
  // else, the key you're trying to remove doesn't exist. Return null.
  else {
    return null;
  }
};

/* hashOne helper function
 * has parameters of key and limit
 * and returns a number within the bounds of 0, limit
 * it will always return the same number for the same key and limit.
*/

/* hashTwo helper function
 * has parameters of key and limit
 * and returns a number within the bounds of 0, limit
 * it will always return the same number for the same key and limit.
*/

/* checkSize helper function
 * this would be a checkSize method of our HashTable constructor. It 
 * would compare the total values with the hash table limit, and if
 * total values < 25% of limit, it would implement the reduce size 
 * helpers function (below).
 * If total values > 75% of limit, it would implement the increase
 * size helper function (also below).
*/

/* reduce size helper function
 * this function would create a copy of the hash table's storage
 * and then change the limit to be half of the previous limit, and
 * reset the number of values to 0. It would then insert all of
 * the values in the old storage property, creating a new hash table
 * with half the size and all of the properties.
*/

/* increase size helper function
 * this function would go through the same process as reduce size,
 * but it would double the size property.
*/{% endhighlight %}

##Conclusion
I hope the code and psuedocode above helped to illuminate the concepts I wrote about last week! Please email me with any further questions or comments (my email address can be found at the bottom of any page on this website).