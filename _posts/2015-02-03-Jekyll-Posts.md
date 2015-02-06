---
layout: post
date: 2015-02-03 16:00:00
title: Jekyll Post Sorting
tags: [Jekyll]
---

___Edit:___ I've realised that because of me changing the filenames to include an extra serial number, the URLs of the post now also include the serial number. Perhaps there is more magic than I thought. Shall update this post when I find the fix for this.

Following the [tutorial](https://github.com/ksami/ksami.github.io/blob/master/README.md/) for setting up Jekyll Now, I named the files for each of my posts using the format year-month-day-title.md. Automagically, this blog would show posts sorted by date.

However, there was this annoyance where if you wrote more than 1 post in a day, Jekyll would not sort the posts by time. This resulted in some messed up post order, as some of you may have previously seen on this blog.

Then, I noticed that within posts written on the same day, Jekyll sorted those posts by alphabetical order. That's when I had an epiphany: Jekyll has always been sorting all posts alphanumerically according to ASCII order (ie. 0-9 before A-Z). That magic wasn't magic at all!

So now I name the files for each of my posts using the format year-month-day-serialnumber-title.md and (insert overused exclamation) my posts are now sorted in the order I wrote them! Of course, if you want to keep track of the time you wrote them, you can always insert hours, or minutes, or seconds, or milliseconds; who knows how fast some of you blog out there.

Note: Okay this might have been obvious to some but it certainly wasn't to me!