---
layout: post
date: 2015-01-30 22:00:00
title: HTML5 Canvas Focus
tags: [muddy, HTML5, canvas, javascript, jquery, KineticJS]
---

Apparently the HTML5 `canvas` element cannot acquire focus which, when trying to capture keyboard events in [KineticJS](http://kineticjs.com) makes it impossible. KineticJS, by the way, only supports the following events, according to its [documentation](http://agavestorm.com/kineticjs/index.html):  
> KineticJS supports mouseover, mousemove, mouseout, mouseenter, mouseleave, mousedown, mouseup, click, dblclick, touchstart, touchmove, touchend, tap, dbltap, dragstart, dragmove, and dragend events.
>
> Kinetic Stage supports contentMouseover, contentMousemove, contentMouseout, contentMousedown, contentMouseup, contentClick, contentDblclick, contentTouchstart, contentTouchmove, contentTouchend, contentTap, and contentDblTap.

As I've mentioned in an earlier post on KineticJS, layers are rendered as `canvas` elements in HTML. Using a workaround from a StackOverflow [answer](http://stackoverflow.com/a/12887221/4469613), I came up with this:

```javascript
$("canvas").attr("tabindex", 1);

$("#page").focus(function() {
  $("canvas").focus();
});
```
Using JQuery (really makes life easier), I add the `tabindex` attribute to the `canvas`. Doesn't really matter what the tabindex actually is as long as it is an integer. Next, I add a callback function for the `focus` event on my `page` element which I use as a container for the entire page. Inside the function, I simply switch focus over to the `canvas` element. From there on, the `.keydown()` function on the `canvas` element will actually work and you can use it to fire off KineticJS events.