---
layout: post
date: 2015-02-15 23:20:00
title: Lookup - Javascript
tags: [programming, Javascript]
---
#### Javascript Objects - Part 2

Continued from [Part 1 - Objects and JSON](http://ksami.github.io/2015/02/15/Objects-and-JSON.html)

Following from the previous example, let's say we are only interested in the actions a user can take

```javascript
var actions = [
    {text: "Move", x: 0, y: 0},
    {text: "Attack", x: 50, y: 0},
    {text: "Skill", x: 0, y: 20},
    {text: "Item", x: 50, y: 20}
];
```
What if we wanted to find the property `x` of the `Move` action? We could try defining `actions` as an object instead of an array like so

```javascript
var actions = {
    move: {text: "Move", x: 0, y: 0},
    attack: {text: "Attack", x: 50, y: 0},
    skill: {text: "Skill", x: 0, y: 20},
    item: {text: "Item", x: 50, y: 20}
};
```

and you would then be able to use the `actions` object as a lookup to find `x` using

```javascript
var x = actions.move;
```
Although this works, there is a duplication of text which may lead to bugs if the code is modified later on. Another limitation is that you would be restricted to only looking up based on 1 variable.

A more flexible and extendable approach would be to define a second object to use as a lookup. Using the `actions` array from the first code example,

```javascript
var lookup = {};
for(var i=0; i<actions.length; i++) {
    lookup[actions[i].text] = actions[i].x;
}

// lookup = {
//     Move: 0,
//     Attack: 50,
//     Skill: 0,
//     Item: 50
// }
```
You can then use the `lookup` object to find `x` using

```javascript
var x = lookup.Move;
//or
var x = lookup["Move"];
```
The `lookup` object can also be modified to look up based on both the `x` and `y` properties to find the `text` property

```javascript
var lookup = {};
for(var i=0; i<actions.length; i++) {
    // if the property does not exist yet
    if(!lookup.hasOwnProperty(actions[i].x)) {
        lookup[actions[i].x] = {};
    }
    lookup[actions[i].x][actions[i].y] = actions[i].text;
}

// lookup = {
//     0: {
//         0: "Move",
//         20: "Skill"
//     },
//     50: {
//         0: "Attack",
//         20: "Item"
//     }
// }
```
You can then use the `lookup` object like so

```javascript
var text = lookup[x][y];
```
This method of creating another object for looking up can also be applied to simple encoding/decoding schemes such as

```javascript
var encoder = {
    e: "a",
    h: "c",
    l: "r",
    o: "y"
}

var decoder = {};
for(var key in encoder) {
    decoder[encoder[key]] = key;
}
```

```javascript
var plainText = ["h", "e", "l", "l", "o"];

var encryptedString = [];
for(var i=0; i<plainText.length; i++) {
    encryptedString[i] = encoder[plainText[i]];
}
// encryptedString = ["c", "a", "r", "r", "y"];

var decryptedString = [];
for(var i=0; i<encryptedString.length; i++) {
    decryptedString[i] = decoder[encryptedString[i]];
}
// decryptedString = ["h", "e", "l", "l", "o"];
```