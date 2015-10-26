---
layout: post
date: 2015-09-05 23:09:11
title: KeystoneJS, ExpressJS and Socket.IO
tags: [programming, javascript, NodeJS, KeystoneJS, SocketIO, ExpressJS]
---

__EDIT__ 2015/09/16: I realised adding event listeners in middleware is not a smart thing to do (hint: middleware gets called on every request) and have therefore updated this guide. Also, I have added session sharing between Express and Socket.IO for accessing stuff like `req.params.id` from Socket.IO.

To integrate [Socket.IO](http://socket.io/) with [KeystoneJS](http://keystonejs.com/) requires some work but it can be done, at least using the packages `keystone 0.3.14` and `socket.io 1.3.6` from `npm`.

The steps below assume you have followed the [Getting Started](http://keystonejs.com/docs/getting-started/) guide and have a folder structure as stated in the guide. `./web.js` is the entry point for the app, `./routes/views/index.js` is the route controller for index and `./templates/views/index.jade` is the jade template rendered for index.

---

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
    },
    onStart: function(){
        var io = keystone.get('io');
        var session = keystone.get('express session');

        // Share session between express and socketio
        io.use(function(socket, next){
            session(socket.handshake, {}, next);
        });

        // Socketio connection
        io.on('connect', function(socket){
            console.log('--- User connected');
            
            // Set session variables in route controller
            // which is going to load the client side socketio
            // in this case, ./routes/index.js
            console.log(socket.handshake.session);
            socket.emit('msg', socket.handshake.session.message);


            socket.on('disconnect', function(){
                console.log('--- User disconnected');
            });
        });
    }
});
```

Set the session variable in `./routes/views/index.js` by adding into the `module.exports` function

```js
req.session.message = 'Hello World';
```



Load `/socket.io/socket.io.js` on the __client's side__ by adding script tags to `./templates/views/index.jade`

```js
<script src="/socket.io/socket.io.js"></script>
<script>

    var socket = io();
    socket.on('msg', function(message){
        console.log(message);
    });

</script>
```



Now to check if Socket.IO is working and session sharing between Express and Socket.IO is enabled, start the app with `node ./web.js` and connect to the server from the client. You should see on the server console

```
--- User connected
```

and on the browser console

```
'Hello World'
```
