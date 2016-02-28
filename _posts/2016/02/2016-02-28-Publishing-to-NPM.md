---
layout: post
date: 2016-02-28 14:45:45
title: Publishing to npm
tags: [npm, nodejs]
---

An npm module is a bunch of code which can be `require`d in NodeJS like 

```js
var fs = require('fs');
```

when importing the native filesystem module, and for browsers in a million other ways which I shall not touch on in this post. Publishing a module to [npm](https://www.npmjs.com/) is quite simple and is a way to give back to the community and/or get into open source.

<br>

The first thing to take note is that your project must have a `package.json` file. This file stores your project settings which npm will use when installing and publishing. The easiest way to ensure all the required fields are there, just run

```sh
npm init
```

in your project directory and follow through the instructions; more details on the `package.json` file found [here](https://docs.npmjs.com/files/package.json).

<br>

Second thing to note is to make sure your package can be installed by npm and `require`d, run

```sh
npm install -g .
```

in the directory which you are developing the module. Then `require` it in another project and test it out.

<br>

Third is to have a license for your project. Github's [Choose A License](http://choosealicense.com/) can help with choosing and applying a license. The MIT license seems to be very popular though.

<br>

Fourth is to add a `README.md` to your project to tell users what the package is about, how to install and how to use the package. Additionally, you can include instructions for developers who want to contribute to your project on how to build and test the project. A sample `README.md` goes something like this

    # Package Name
    Description of package

    ## Install
    ```
    npm install package-name
    ```

    ## Usage
    ```javascript
    // Sample code
    ```

    ## Build
    Instructions on how to build

    ## Test
    Instructions on how to test

<br>

Finally, create a user account on the npm site, link it using

```sh
npm adduser
```

then publish your package using

```sh
npm publish
```

and you are done!