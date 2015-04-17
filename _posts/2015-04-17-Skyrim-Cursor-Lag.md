---
layout: post
date: 2015-04-17 17:33:06
title: Skyrim cursor lag
tags: [Skyrim]
---
Just a quick post on a Skyrim fix I've found. In Skyrim for PC, many people have experienced lag with the mouse and the most popular answer is to add `IPresentInterval=0` and `bMouseAcceleration=0` to your `SkyrimPrefs.ini`. It didn't work for me though no matter how many times I deleted, retyped that line, saved the file and restarted the game.

What works though is this:

1. In-game go to the Settings menu
2. Select General
3. Look for Xbox 360 Controller or something like that and uncheck the box

Pretty dumb to have that option on by default; probably something left over from the port.