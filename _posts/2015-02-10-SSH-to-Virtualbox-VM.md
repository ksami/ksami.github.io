---
layout: post
date: 2015-02-10 00:10:00
title: SSH to VirtualBox VM
tags: [Arch, Linux, network, VirtualBox, VM, Windows, SSH]
---

#### Adventuring with my Arch VM Part 2

Setting up SSH access between your VM and host allows for control of the VM from the host itself. Could be useful for stuff like getting familiar with SSH, cryptography and hacking. I use SSH so I can continue working from my Windows environment but use Linux tools, eg. Meteor only runs on Linux. My setup is ArchLinux VM in VirtualBox on Win8 host and I use Cygwin on Windows.

__Note__: This post is on SSH and is not limited to VMs, VirtualBox, Linux or Windows; eg. Github uses SSH to authenticate users. (More on Github and version control another time.)

There are two parts to security in SSH, the public key and the private key, and they go hand-in-hand. If you are interested to learn more, search for public key cryptography. More importantly, the private key is and should be kept private ie. not for anyone else but you. Please keep in mind the distinction between the public and private keys.

---

(This assumes that a network already exists between the two machines. For ArchLinux VirtualBox VM and Windows host, see my [previous post](http://ksami.github.io/2015/02/09/Host-Only-Network.html))

Firstly, make sure you have SSH installed on both machines. Arch should already have SSH installed via the `openssh` package. On Windows using Cygwin, look for SSH under Cygwin's package repositories.

On Arch, start the sshd service using `sudo systemctl start sshd`. Alternatively, enable start on every boot using `sudo systemctl enable sshd`.

On Cygwin, generate the SSH keys using `ssh-keygen -t rsa` and follow the prompts. The password here will be the password to key in whenever SSH wants to use your SSH keys; it is also possible to not have a password, just don't key in one at this step.

The generated SSH keys will be located by default at `~/.ssh`. In there you will see 2 files, the public key `id_rsa.pub` and the private key `id_rsa`. Use the command `ssh-copy-id yourusername@hostname`, eg. `ssh-copy-id ksami@192.168.56.2`. This command will copy your public key `id_rsa.pub` from your local machine to your remote machine, in this case to `~/.ssh` for the user `ksami` at `192.168.56.2`.

Finally, test that SSH is working by using the command `ssh yourusername@hostname` to access the remote machine.

If you get a message with something about `WARNING: UNPROTECTED PRIVATE KEY FILE!` or `Permissions too open`, use the command `chmod 600 ~/.ssh/id_rsa` to set more restrictive permissions on your private key file.