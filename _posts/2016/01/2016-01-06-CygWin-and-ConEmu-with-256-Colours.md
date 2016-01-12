---
layout: post
date: 2016-01-06 14:23:59
title: Cygwin and ConEmu with 256 Colours
tags: [Cygwin, ConEmu, MinTTY, console, terminal, ricing, linux, windows]
---

For those who do not know, [Cygwin](https://www.cygwin.com/) is "a large collection of GNU and Open Source tools which provide functionality similar to a Linux distribution on Windows", [MinTTY](https://code.google.com/p/mintty/) is the default terminal installed by Cygwin, and [ConEmu](http://conemu.github.io/en/) is a "Windows console emulator with tabs" with a customisable GUI.

Cygwin is a great tool for those who are getting used to the Linux command line, and also for those who are on Windows and missing Linux command line functionality.

Getting Cygwin and ConEmu to work together with full 256-colour support takes a bit of configuration though. According to [ConEmu's documentation](http://conemu.github.io/en/CygwinAnsi.html), Cygwin processes [ANSI sequences](https://en.wikipedia.org/wiki/ANSI_escape_code), the codes for formatting output text in terminals, internally and does not send them to ConEmu. Cygwin and MinTTY work well together though with full 256-colour support.

In that same document, ConEmu's author, Maximus5, an experimental approach using a work-in-progress tool but I could not get it to work. Instead, I hooked up ConEmu with Cygwin's MinTTY to get the effects I wanted.


##### MinTTY
Firstly, MinTTY must be configured to display 256 colours. This can be done by going to MinTTY's Options > Terminal and changing Type to `xterm-256color`.

![MinTTY Options](/images/2016-01-06-options.png)

You can test if 256 colours has been enabled by using the command:

```
tput colors
```

or with this [script](https://github.com/ksami/dotfiles/blob/master/256colortest.py) which will output all 256 colours.


##### ConEmu
Next will be to configure ConEmu to launch MinTTY. Under ConEmu's Settings > Startup > Tasks, create a new task for starting MinTTY with its default configuration and with the home directory as starting directory. Take note of the name of the task, in this case, it is `{Zsh::CygWin MinTTY}`.

```
set CHERE_INVOKING=1 & \windows\path\to\mintty.exe -  -new_console:d:\windows\path\to\home\directory
```

![ConEmu settings](/images/2016-01-06-conemu-tasks.png)

Finally, go to the properties of the ConEmu shortcut by right-clicking the shortcut, and under Shortcut > Target, append `/cmd Your Task Name` to the path. For example, if under Target, the path to my ConEmu64.exe is `"D:\ConEmu\ConEmu64.exe"` and my task name is `{Zsh::CygWin MinTTY}`, then I would change Target to 

```
"D:\ConEmu\ConEmu64.exe" /cmd {Zsh::CygWin MinTTY}
```

![ConEmu properties](/images/2016-01-06-conemu-properties.png)

<br><br>

And that's it! Enjoy your new 256 colour terminal!

[![Desktop](/images/2016-01-06-desktop.png)](/images/2016-01-06-desktop.png)