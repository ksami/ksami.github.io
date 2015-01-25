---
layout: post
title: posts[1] = "First Post - Markdown";
tags: credits markdown
---
First post! Welcome reader!
(I love stories which break the fourth wall and so will be pathetically and haphazardly attempting to do this every now and then)  
  
Time for some credits. This blog was created using:  
- the static website generator [Jekyll]
- using [Jekyll Now] as a starting point
  
---  
And on to our first post proper:
### Intro to Markdown
All posts using [Jekyll] are written in Markdown.
Markdown is a markup language using plain-text syntax that can be converted into HTML and yet still easy for humans to read.
However, I am still not quite familiar with the syntax and so shall be practising it here in my first post under the guise of teaching you, honourable reader.

Credits to [Markdown Cheatsheet]

#### Contents
- [Headers](#headers)
- [Formatting](#formatting)
- [Lists](#lists)
- [Links](#links)
- [Images](#images)
- [Code](#code)
- [Blockquote](#blockquote)
- [Horizontal Rules](#horizontalrules)
- [Line Breaks](#linebreaks)


#### <a name="headers"></a>Headers
```
# H1
## H2
...
###### H6
```
# H1
## H2
...
###### H6


#### <a name="formatting"></a>Formatting
```
*italics* or _italics_  
**bold** or __bold__  
~~strikethrough~~
```
*italics* or _italics_  
**bold** or __bold__  
~~strikethrough~~


#### <a name="lists"></a>Lists
```
* Unordered
- list
+ item

1. Ordered
5. list
  1. Ordered
  2. sub item
4. item
  - Unordered
```
* Unordered
- list
+ item

1. Ordered
5. list
  1. Ordered
  2. sub item
4. item
  - Unordered


#### <a name="links"></a>Links
```
[Link](http://www.google.com)  
[Link with alt text](http://www.google.com "Alt-text")  
[Reference style link][1]  
[Reference link]  

[1]: http://www.google.com
[Reference link]: http://www.google.com

[Link to location in document](#links)  
Place <a name="links"></a> at location to link  
example demonstrated using this post's contents table
```
[Link](http://www.google.com)  
[Link with alt text](http://www.google.com "Alt-text")  
[Reference style link][1]  
[Reference link]  

[1]: http://www.google.com
[Reference link]: http://www.google.com


#### <a name="images"></a>Images
```
![Alt-text](../images/jekyll-logo.png)  
![Alt-text][Image link]  

[Image link]: ../images/jekyll-logo.png
```
![Alt-text](../images/jekyll-logo.png)  
![Alt-text][Image link]  

[Image link]: ../images/jekyll-logo.png


#### <a name="code"></a>Code
    
    Inline `code`
    
    ```javascript
    var string = "code with Javascript syntax highlighting";
    console.log(string);
    ```
        //code block using 4 spaces indentation
        alert(1);


Inline `code`

```javascript
var string = "code with Javascript syntax highlighting";
console.log(string);
```

    //code block using 4 spaces indentation
    alert(1);



#### <a name="blockquote"></a>Blockquote

```
> Greentext
> also known as blockquote
```

> Greentext
> also known as blockquote


#### <a name="horizontalrules"></a>Horizontal Rules

```
---
___
***
```
---
___
***


#### <a name="linebreaks"></a>Line Breaks

```
Without two spaces at the end,
this is still on the same line

Force a line bre  
ak with two spaces at the end
```
Without two spaces at the end,
this is still on the same line

Force a line bre  
ak with two spaces at the end

<br>
I have tried to keep it as short as possible only including the most frequently used syntax.
Any other details can be found at [Markdown Cheatsheet] or simply searching for __Markdown Syntax__.

<br><br>` - ksami`

[Jekyll]: http://github.com/jekyll/jekyll
[Jekyll Now]: http://github.com/barryclark/jekyll-now
[Markdown Cheatsheet]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet