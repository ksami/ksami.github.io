---
layout: post
date: 2015-08-24 23:40:00
title: sudo repeat
tags: [bash, zsh, shell, programming]
---

Anyone who has used a command line shell on a UNIX system has surely come across a situation with a forgotten `sudo`. Well, I finally got annoyed enough to look for a solution, an alias to repeat the last command with `sudo` prepended. And of course, I used `please` as the alias since I am such a nice person.

For Bash, add the following line/s to `~/.bashrc`:
```
alias pls='sudo $(fc -s)'
alias please='sudo $(fc -s)'
```

For Zsh, add the following line/s to `~/.zshrc`:
```
alias pls='sudo $(fc -ln -1)'
alias please='sudo $(fc -ln -1)'
```