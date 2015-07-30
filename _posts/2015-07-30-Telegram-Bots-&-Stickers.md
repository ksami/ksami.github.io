---
layout: post
date: 2015-07-30 14:36:57
title: Telegram Bots & Stickers
tags: [Telegram]
---
TL;DR: Links to bots and stickers at the bottom of this post.


[Telegram](https://telegram.org/) is a chatting app similar to Whatsapp but synced between your phone, tablet, laptop and whatever other device you have.

Telegram recently came up with support for developers to create bots, functionally similar to [IRC bots](https://en.wikipedia.org/wiki/IRC_bot). In short, bots help you to do things through the chatting app.

I first came up with a toy bot that returns a random number to get used to the [Telegram Bot API](https://core.telegram.org/bots/api). The workflow is simple:

1. Request the Bot API for a list of messages/updates that you have yet to process
2. Parse the messages/updates
3. Perform a corresponding action
4. Request the Bot API to return a reply

Once I got used to this workflow, I refactored the entire code for the toy bot to support a structure where steps 2 and 3 can be easily switched out. Code is on my github repo [telegrambot](https://github.com/ksami/telegrambot).


Bots:

1. [Meetup Bot](http://telegram.me/meetup_bot)  
2. [ksami_bot (toy bot)](http://telegram.me/ksami_bot)  

Stickers:  

1. [Crayon Pop](https://telegram.me/addstickers/CrayonPop)
2. [AoA Mina](https://telegram.me/addstickers/AoaMina)
3. [SNSD Taeyeon](https://telegram.me/addstickers/snsdtaeyeon)
4. [One Piece - Chopper](https://telegram.me/addstickers/OnepieceChopper)
5. [ksami](https://telegram.me/addstickers/ksami)  
