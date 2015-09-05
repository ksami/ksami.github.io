---
layout: post
date: 2015-09-05 23:09:11
title: KeystoneJS and Socket.IO
tags: [programming, javascript, NodeJS, KeystoneJS, SocketIO]
---

To integrate [Socket.IO](http://socket.io/) with [KeystoneJS](http://keystonejs.com/) requires some work but it can be done, at least using the packages `keystone 0.3.14` and `socket.io 1.3.6` from `npm`.

The steps below assume you have followed the [Getting Started](http://keystonejs.com/docs/getting-started/) guide and have a folder structure as stated in the guide. `./web.js` is the entry point for the app, `./route/middleware.js` is where middleware is declared, `./routes/index.js` is the route index controller.


Firstly, `npm install --save socket.io` to get Socket.IO and update your `./package.json`.

Change the following in `./web.js`

```js
//replace this
keystone.start();

//with this
var socketio = require('socket.io');
keystone.start({
    onHttpServerCreated: function(){
        keystone.set('io', socketio.listen(keystone.httpServer));
    }
});
```

Create a custom middleware to handle Socket.IO events by adding the following in `./route/middleware.js`

```js
/**
 *   Socketio server-side events
 */
exports.handleSocketio = function(req, res, next) {
    var io = keystone.get('io');

    io.on('connection', function(socket){
        console.log('--- User connected');

        socket.on('disconnect', function(){
            console.log('--- User disconnected');
        });
    });

    next();
};
```

Load the custom middleware by adding the following in `./routes/index.js`
```js
keystone.pre('routes', middleware.handleSocketio);
```

Load `/socket.io/socket.io.js` on the __client's side__ by adding a script tag
```html
<script src="/socket.io/socket.io.js"></script>
```

Lastly, initialise Socket.IO on the __client's side__ by adding
```js
var socket = io();
```

Now to check if Socket.IO is working, start the app with `node ./web.js` and connect to the server from the client. You should see on the server console

```
--- User connected
```