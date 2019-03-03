---
layout: post
date: 2019-02-15 22:42:31
title: Keeping a process alive after ending SSH session
tags: [ssh]
---

Typical scenario:

1. SSH to a server
2. Start a long-running process eg. webserver, utility script
3. End SSH session
4. Long-running process gets killed

Ending an SSH session usually kills all processes started during the session.

In order to keep the process alive even after the session is ended, there are a few methods:

### nohup
The easiest method is to just start your process with [`nohup`](https://ss64.com/bash/nohup.html) which causes the process to ignore `SIGHUP`, the hangup signal, fired to processes when the SSH session ends.
```
nohup ./script.sh &
```

### disown
If your process is already running, you can suspend it using `Ctrl-Z` then
```
bg
disown
```
to resume running the suspended task in the background, and [`disown`](https://ss64.com/bash/syntax-jobs.html) to remove ownership from the SSH session which allows the task to keep running.

### screen
[`screen`](https://ss64.com/bash/screen.html) is commonly used for managing terminal sessions; it includes a "detached" mode which we can use to detach a session from the SSH session.
```
screen -S session-name
```
will launch a new screen where you can start your process.
`Ctrl-A Ctrl-D` will detach the screen and you can end the SSH session after this. If you want to reattach
```
screen -r session-name
```
