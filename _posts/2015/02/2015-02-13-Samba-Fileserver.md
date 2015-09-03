---
layout: post
date: 2015-02-12 22:00:00
title: Samba Fileserver
tags: [Arch, Linux, network, VirtualBox, VM, Windows, Samba]
---

#### Adventuring with my Arch VM Part 3

I also set up the Samba fileserver on my Arch VM to work directly on files from Arch on the host. It would be a lot simpler than having to git push to the VM everytime I needed to test. I currently use this method to work on MeteorJS code from my Win8 environment. My setup is Arch VM running in VirtualBox on a Win8 host.

---
(This assumes that a network already exists between the two machines. For ArchLinux VirtualBox VM and Windows host, see my [previous post](http://ksami.github.io/2015/02/09/Host-Only-Network.html))

First step is of course to install Samba `sudo pacman -S samba`.

Enable Samba to run on system boot using `sudo systemctl enable smbd nmbd`.

The config file is located at `/etc/samba/smb.conf`, a sample file located at `/etc/samba/smb.conf.default`. I used the one from the [ArchWiki](https://wiki.archlinux.org/index.php/Samba/Tips_and_tricks#Sample_Passwordless_Configuration). Change interfaces to the one configured for your host-only network. I also added in a line for `hosts allow` to enable access for my Win8 host.

```
# Only showing lines I changed from the sample from ArchWiki

[global]
    ...

    # host-only network, obtained from ifconfig on Arch
    interfaces = 192.168.56.2/24

    # allow host but deny any other connection, obtained from ipconfig on Windows
    hosts allow = 192.168.56.1
    hosts deny = ALL

[share]
   path = /shares
   public = yes
   only guest = yes
   writable = yes
```

Run `testparm` after creating/modifying the `smb.conf` file to check for syntax errors.

`sudo systemctl restart smbd nmbd` to reload the config file.

On Windows, you can now connect to `/shares` by opening File Explorer and navigating to Arch's IP address and the name in the square brackets the config file  eg. `\\192.168.56.2\share`.

To make it permanent, use network drives to map the address to a drive letter in Explorer. In File Explorer, right-click on Network and select Map Network Drive... and fill in the fields.

![mapnetworkdrive](../../../images/mapnetworkdrive.png)

Now you can access files on Arch as though they were on another drive in Windows. Sharing other directories on Arch is also possible by changing the `path` key in the config file. But remember that parent directories as well as the target directory need to have the proper permissions to be accessible. For other config options, have a look at the [man page](https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html) for smb.conf.