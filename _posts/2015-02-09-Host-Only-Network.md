---
layout: post
date: 2015-02-09 22:27:00
title: Host-Only Network
tags: [Arch, Linux, network, VirtualBox, VM, Windows]
---

#### Adventuring with my Arch VM Part 1!

A host-only network can be set up between the VM running in VirtualBox and the host machine for communication solely between these two. This is separate from the NAT already configured by VirtualBox for internet access to your VM. My setup is a Win8 host running ArchLinux.

---

Firstly, in VirtualBox Manager, go to your machine's settings > Network > Adapter 2 and enable it as a host-only network adapter.

![hostonlynetwork1](../../../images/hostonlynetwork1.png)

Open up a Command Prompt on Windows (Win+R, "cmd", Enter) and run the `ipconfig` command. Look for `Ethernet adapter VirtualBox Host-Only Network` in the output of the command and take note of the IPv4 address: 

```
Ethernet adapter VirtualBox Host-Only Network:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::d972:6a24:3004:c2ae%8
   IPv4 Address. . . . . . . . . . . : 192.168.56.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :
```

Start up Arch, use `ip addr` or `ifconfig` to check for the new network adapter's name, it will be the one with a `DOWN` flag; in my case it's `enp0s8`.

Then create a new network profile `sudo nano /etc/netctl/hostonly_network` with the following contents; __replacing__ `enp0s8` with your network adapter's name and `Address` with an IPv4 address in the same domain as the one you obtained in the `ipconfig` step:

```
Description='VirtualBox Host-Only Network'
Interface=enp0s8
Connection=ethernet
IP=static
Address=('192.168.56.2/24')
```

Finally, use `sudo netctl enable hostonly_network` to enable the new network profile and `reboot` your machine. Once your machine is up, test the network by pinging the IP address you entered into the network profile, *not* the one you obtained using `ipconfig`. If all goes well, you can continue on to start fileservers or SSH on the VM. I will be detailing the steps for SSH and the Samba fileserver next time.
