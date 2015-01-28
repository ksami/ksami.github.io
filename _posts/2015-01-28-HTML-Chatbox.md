---
layout: post
title: HTML Chatbox
tags: [muddy, programming, javascript, JQuery]
---

One of the most important features in a multi-user environment is the ability to communicate with other users. Implementing a chat box in html using javascript isn't very difficult.

___TL;DR___: Code at [bottom](#bottom) of post. [Demo](#demo) below the bottom.

First, begin by creating an unordered list `<ul>` element to store all the messages.

```html
<ul id="messages"></ul>
```

A CSS rule for the list can be added to take away the bullet points shown on each list item `<li>`.

```css
<style type="text/css">
  #messages {
    /* Specify type of bullet point */
    list-style-type: none;

    /* Un-indent list items */
    padding-left: 0;

    /* Set height of list to 4*font-size */
    height: 4em;

    /* Force scrolling when contents in this element exceed its boundaries */
    overflow: auto;
  }  
</style>
```

Then create the `<input>` textbox and containing `<form>`.

```html
<form id="input">
  <input id="userInput" />
</form>
```

Using the insanely popular JQuery library for javascript, get the text keyed in by the user, create an `<li>` element and append it to the previously created `<ul>`. An important point to note is to use the JQuery method `text()` to set the text in the newly created `<li>` element. This would escape any HTML input to prevent JavaScript/HTML injection, a gateway to [Cross-Site Scripting (XSS)](http://en.wikipedia.org/wiki/Cross-site_scripting) attacks.

```html
<!-- Include this somewhere before the following script -->
<script src="https://code.jquery.com/jquery-1.11.2.min.js"></script>
```

```javascript
<script>
  function addMessage() {
    // Get the text from the input textbox
    var text = $("#userInput").val();

    // Add a new <li> element with the contents of text
    $('#messages').append($('<li>').text('> '+ text));

    // Clear input textbox when done
    $("#userInput").val("");
  }
</script>
```

Also, attach the event listener for the `submit` event on to the `<form>` when the document has finished loading. `preventDefault()` is needed to prevent the default action taken by the browser for that element. In this case the default action for the `<form>` element is a redirect by the browser; something we don't want to happen while chatting in the chat box.

```javascript
<script>
  // Function in argument is called when the HTML document is ready
  $(document).ready(function(){

    // Event handler for when the enter key is pressed in the input textbox
    $("#input").submit(function(event){

      // Adds the message to the list
      addMessage();

      // Prevent browser's default action for this event
      event.preventDefault();
    });
  });
</script>
```

One more thing to consider is that using this approach of adding `<li>` elements to a `<ul>`, the number of DOM elements on your page can get very large (>100K). There isn't a specific maximum number a browser can handle but I'd say a 1000 should be more than sufficient. Adjust the buffer based on your needs.

The code below waits till the number of `<li>` elements exceed 2 times the specified buffer then removes the oldest specified buffer number of `<li>` elements. This approach simulates a neverending list (as long as the user doesn't scroll beyond the buffer) and implements a maximum limit for the list without carrying out a `remove()` operation everytime an `<li>` is added which could be costly.

```javascript
<script>
  // (Number of <li> elements allowed in the list) / 2
  // small for demonstration's sake but usually large
  // determine based on your needs, I use 100
  var buffer = 3;

  // If the number of <li> elements is more than buffer * 2
  if( $("#messages li").length >= buffer*2) {

    // Remove oldest buffer no. of elements
    $("#messages li").slice(0,buffer).remove();
  }

  // Scroll the list to the bottom (newest message)
  $("#messages").scrollTop($("#messages")[0].scrollHeight);
</script>
```

###### <a name="bottom"></a>Bottom
All the code used is put together and included on the page for the [demo](#demo).

```css
<!--CSS-->

<style type="text/css">
  #messages {
    /* Specify type of bullet point */
    list-style-type: none;

    /* Un-indent list items */
    padding-left: 0;

    /* Set height of list to 4*font-size */
    height: 4em;

    /* Force scrolling when contents in this element exceed its boundaries */
    overflow: auto;
  }  
</style>
```

```html
<!--HTML-->

<ul id="messages"></ul>

<form id="input">
  <input id="userInput" />
</form>
```

```javascript
<!--Scripts-->

<script src="https://code.jquery.com/jquery-1.11.2.min.js"></script>

<script>
  // Function in argument is called when the HTML document is ready
  $(document).ready(function(){

    // Event handler for when the enter key is pressed in the input textbox
    $("#input").submit(function(event){

      // Adds the message to the list
      addMessage();

      // Prevent browser's default action for this event
      event.preventDefault();
    });
  });

  function addMessage() {
    // (Number of <li> elements allowed in the list) / 2
    // small for demonstration's sake but usually large
    // determine based on your needs, I use 100
    var buffer = 3;

    // Get the text from the input textbox
    var text = $("#userInput").val();

    // Add a new <li> element with the contents of text
    $('#messages').append($('<li>').text('> '+ text));

    // If the number of <li> elements is more than buffer * 2
    if( $("#messages li").length >= buffer*2) {

      // Remove oldest buffer no. of elements
      $("#messages li").slice(0,buffer).remove();
    }

    // Scroll the list to the bottom (newest message)
    $("#messages").scrollTop($("#messages")[0].scrollHeight);

    // Clear input textbox when done
    $("#userInput").val("");
  }
</script>
```

###### <a name="demo"></a>Demo
Try it for yourself here. This is the exact code shown above. Enter a few (>6) messages into the textbox below.


<!--CSS-->

<style type="text/css">
  #messages {
    /* Specify type of bullet point */
    list-style-type: none;

    /* Un-indent list items */
    padding-left: 0;

    /* Set height of list to 4*font-size */
    height: 4em;

    /* Force scrolling when contents in this element exceed its boundaries */
    overflow: auto;
  }  
</style>


<!--HTML-->

<ul id="messages"></ul>

<form id="input">
  <input id="userInput" />
</form>


<!--Scripts-->

<script src="https://code.jquery.com/jquery-1.11.2.min.js"></script>

<script>
  // Function in argument is called when the HTML document is ready
  $(document).ready(function(){

    // Event handler for when the enter key is pressed in the input textbox
    $("#input").submit(function(event){

      // Adds the message to the list
      addMessage();

      // Prevent browser's default action for this event
      event.preventDefault();
    });
  });

  function addMessage() {
    // (Number of <li> elements allowed in the list) / 2
    // small for demonstration's sake but usually large
    // determine based on your needs, I use 100
    var buffer = 3;

    // Get the text from the input textbox
    var text = $("#userInput").val();

    // Add a new <li> element with the contents of text
    $('#messages').append($('<li>').text('> '+ text));

    // If the number of <li> elements is more than buffer * 2
    if( $("#messages li").length >= buffer*2) {

      // Remove oldest buffer no. of elements
      $("#messages li").slice(0,buffer).remove();
    }

    // Scroll the list to the bottom (newest message)
    $("#messages").scrollTop($("#messages")[0].scrollHeight);

    // Clear input textbox when done
    $("#userInput").val("");
  }
</script>