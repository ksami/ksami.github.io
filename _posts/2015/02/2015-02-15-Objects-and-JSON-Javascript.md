---
layout: post
date: 2015-02-15 22:02:00
title: Objects and JSON - Javascript
tags: [programming, Javascript, JSON]
---
#### Javascript Objects - Part 1

In Javascript, there is a data structure known as an object, or dictionary in other languages eg. Python. An object is defined using key-value pairs as such: 

```javascript
var obj = {
    key1: value1,
    key2: value2,
    key3: value3
};
```

where keys are strings and values can be of any type.

To find the value for a particular key, you would then use

```javascript
var value = obj[key];
// or
var value = obj.key;
```
To modify the value for a particular key or create the key-value pair if it doesn't exist

```javascript
obj[key] = value;
// or
obj.key = value;
```
An application of this is the popular [JSON](http://www.w3schools.com/json/) format which specifies an easy to use syntax for storing data. For example, to store properties of a User Interface (UI) menu item, you could save a JSON file with

```javascript
{
    "menu": [
        {"text": "Move", "x": 0, "y": 0},
        {"text": "Attack", "x": 50, "y": 0},
        {"text": "Skill", "x": 0, "y": 20},
        {"text": "Item", "x": 50, "y": 20}
    ]
}
```
For simplicity's sake, we will assume that the JSON data has been loaded into a Javascript variable `jsonstring`. To convert the JSON data into a Javascript object

```javascript
var obj = JSON.parse(jsonstring);
```

and you can then operate on the object as usual.

On to [Part 2: Lookup ->](http://ksami.github.io/2015/02/15/Lookup-Javascript.html)