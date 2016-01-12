---
layout: post
date: 2016-01-07 23:08:43
title: Testing Javascript with Mocha and Chai
tags: [testing, Javascript, Node.js, Mocha, Chai]
---

Continuation from the [previous post](../../../2015/12/26/Introduction-to-Testing-Javascript-using-Mocha.html).

The basic pattern for tests as I've introduced in the previous post makes use of the `assert` module from Node.js and a boolean condition to test for, eg.

```js
assert(1==='one');
// throws AssertionError
```

Since Mocha will catch any error that has been thrown as an indication that the test has failed, other assertion libraries can be used.

[Chai](http://chaijs.com/) is an assertion library that includes more convenience functions than the basic `assert` module such as `isUndefined`, `include`, `instanceOf` etc. Chai also allows one to write these assertions in different styles which try to make specifying tests more natural.

```js
// Assert style
assert.typeOf(name, 'string');

// Expect style
expect(name).to.be.a('string');

// Should style
name.should.be.a('string');
```

You can read more about the styles [here](http://chaijs.com/guide/styles/). Personally, I prefer the Assert style since it is more similar to how I usually write code.

Installing Chai is just a simple

```
npm install chai
```

using the package manager [npm](https://www.npmjs.com/).

Then, to use the Assert style of writing assertions, use Chai's `assert` instead of the one from Node.js and you are ready to start writing assertions.

```js
var assert = require('chai').assert;
var error;

assert.isUndefined(error);
```

#### Bonus
Chai has a number of [plugins](http://chaijs.com/plugins) which extend Chai's functionality for various use cases. The ones I tried and like so far are the [Chai as Promised](https://github.com/domenic/chai-as-promised) and [chai-subset](https://github.com/debitoor/chai-subset) plugins which extend Chai with assertions for Promises and subsets of objects respectively. Here are two examples for using these plugins.

```js
// Chai as Promised
var fs = require('fs');
var chai = require('chai');
var assert = chai.assert;
chai.use(require('chai-as-promised'));

var p_readFile = new Promise(function(resolve, reject){
    fs.readFile('filename.txt', function(err, data){
        if(err) {reject(err);}
        else {resolve(data);}
    });
});

describe('Read file async', function(){
    it('should successfully read the file', function(){
        return assert.isFulfilled(p_readFile);
    });
});
```

```js
// chai-subset
var fs = require('fs');
var chai = require('chai');
var assert = chai.assert;
chai.use(require('chai-subset'));

var contents = fs.readFileSync('data.json');
var jsonData = JSON.parse(contents);

describe('Get JSON data', function(){
    it('should at least have the necessary values', function(){
        var user = {
            id: 1,
            name: {first: 'Firstname', last: 'Lastname'}
        };
        assert.containSubset(jsonData, user);
    });
});
```
