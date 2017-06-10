---
layout: post
date: 2017-06-10 21:37:05
title: Cross-Origin Communication Between Windows
tags: [web, javascript, cors]
---

As anyone developing for the web, whether backend or frontend, will know, [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) is the source of many headaches; but, to its credit, it's responsible for letting us all browse the web a little safer.

The problem we came across was how to enable communication between a popup and its parent window when the two windows were on different domains. The aim was to close the popup after it has landed on a certain URL after a chain of redirects.

By default, [CORS will prevent the parent window from reading any information about the popup's location](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy#Cross-origin_script_API_access). Hence the following would not work:

```javascript
// on example.org

var popup = window.open("http://example2.org");
console.log(popup.location.href);

// Uncaught DOMException: Blocked a frame with origin "http://example.org" from accessing a cross-origin frame.
```

(Side note: TIL someone bought example2.org and helpfully set up a page that says in H1 "It works!"; thank you stranger!)

By messing around in [Chrome DevTools](https://developer.chrome.com/devtools) and digging around the documentation at [Mozilla Developer Network](https://developer.mozilla.org/en-US/), which I think is still the best source for web/Javascript documentation, we managed to find a method that might fit our use case: [Window.postMessage()](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage). According to the documentation, "`Window.postMessage()` provides a controlled mechanism to circumvent [CORS restrictions] in a way which is secure when properly used".

The `postMessage()` mechanism consists of two parts: a `postMessage()` on the sender's end, and an event listener for `"message"` on the receiver's end. This would mean both sender and receiver would have to be aware and prepared for a message. In addition, there are [a few other checks to enhance security](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage#Security_concerns).

Fortunately, we could control the response from the final destination. Using that, we came up with the solution to return a HTML page with a script tag to execute `postMessage()` as the response:

```javascript
// on example.org

var popup = window.open("http://example2.org");

// Listen for messages
window.addEventListener("message", function(event) {
    // Ignore messages from unexpected origins
    if(event.origin !== "http://example2.org") {
        return;
    }

    if(event.data === "success") {
        popup.close();
    } else {
        // Oh no!
    }
});
```

```javascript
// on example2.org

var parent = window.opener;

// Queues a message to be sent to the parent window if the parent's location is "http://example.org"
parent.postMessage("success", "http://example.org");
```