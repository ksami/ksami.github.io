---
layout: post
date: 2015-12-26 16:42:38
title: Introduction to Testing Javascript using Mocha
tags: [testing, Javascript, Node.js, Mocha]
---
If I wanted to test a function in Javascript, what would I do? The simplest method would be to run the function, manually keying in the inputs and checking if the output is correct. What if I had 10 functions to test? Or what if you wanted to do [Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development)? Doing it all manually and trying to cover all edge cases would definitely be too much to handle; what more when testing all these functions all over again every time a part of the codebase is modified to ensure nothing broke.


#### Rationale
That's where a testing framework comes in. We're programmers and we're lazy. If something can be automated, it should. (*Terms and conditions apply, of course. Don't quote me on this.) Sure, we could use a couple of if-else statements to test if a function actually works. Using the following function taken from this [StackOverflow answer](http://stackoverflow.com/a/1830844/4469613) as an example of a real-world function that would require testing:

```javascript
// Returns true if variable is a number, else returns false
function isNumeric(n) {
  return !isNaN(parseFloat(n)) && isFinite(n);
}
```

The tests for the `isNumeric` function can be written in pure javascript like this:

```javascript
// Tests
if(isNumeric(1)){
    console.log('Passed');
}
else{
    console.log('Fail');
}
//...
```

What if you had a whole lot of tests to carry out? It would look so much nicer if there was some sort of framework to organise these tests and report results in a sensible (and colourful, who doesn't like colour?) fashion. Using [Mocha](https://mochajs.org/), the above tests would look something like this:

```javascript
var assert = require('assert');

describe('isNumeric()', function(){
    it('should return true on numbers', function(){
        assert(isNumeric(1));
    });
    //...
});
```

And the output from Mocha:

![mocha output](../mocha.png)

Or for more fun, Mocha can also format its output in other formats, what Mocha calls reporters. For example, from [Mocha's page](https://mochajs.org/), the nyan reporter produces:

![nyan reporter](https://mochajs.org/images/reporter-nyan.png)


#### Setting up Mocha tests
The easiest way to install [Mocha](https://mochajs.org/) would be using [npm](https://www.npmjs.com/). To install globally on your system, first install npm then, in your terminal, run:

```
npm install -g mocha
```

Then, create a `test` folder in your working directory, the directory where your scripts reside in, then create a new file in `test` with the Mocha tests.


#### Mocha functions
The testing functions Mocha uses, at the very basic level, have a very simple signature. For the `it` function:

```javascript
it('Description of what the test should accomplish', function(){
    // statements which throw an error on failure
});
```

`it` statements are generally encompassed by `describe` to categorise groups of tests together:

```javascript
describe('Description of category of tests', function(){
    // it statements
});
```

`describe` statements can also be encompassed by other `describe`s to describe larger categories like so:

```javascript
describe('General category', function(){
    describe('Specific category', function(){
        // it statements
    });
});
```

Mocha has [other features](https://mochajs.org) eg. setting up and destroying test environments before and after tests, output formats and timeouts but `it` and `describe` are the two basic functions necessary for writing any test.


#### All together now
Note: Do note that I am using Node.js; for browsers, please substitute the Node.js syntax with whatever file/module/bundle loader you are most familiar with, or simply concatenate the files yourself.

With a folder structure as such:

```
.
├── numberChecker.js
└── test
    └──test-checkInput.js
```

And the files:

```javascript
//numberChecker.js

// Returns true if variable is a number, else returns false
function isNumeric(n) {
  return !isNaN(parseFloat(n)) && isFinite(n);
}

//node.js export syntax
module.exports = isNumeric;
```

```javascript
//test-checkInput.js

//node.js import syntax
var isNumeric = require('../numberChecker');
var assert = require('assert');

describe('Input Check', function(){
    describe('isNumeric()', function(){
        it('should return true on numbers', function(){
            //assert throws an error if argument is false
            assert(isNumeric(1));
        });
        it('should return false on strings', function(){
            assert(!isNumeric('a'));
        });
        it('should return false on undefined', function(){
            assert(!isNumeric(undefined));
        });
    });
});
```

To run the tests, run `mocha` in the top-level directory, the directory where `numberChecker.js` is, from your terminal. `mocha` will search for the folder named `test` and run all tests in that folder.

In the next post, I will talk about using Mocha with [Chai](http://chaijs.com/), an assertion library that will make it easier to test for things other than simple true/false values.