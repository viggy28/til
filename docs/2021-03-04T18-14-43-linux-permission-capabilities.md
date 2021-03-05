---
date: 2021-03-04T18:14:43-08:00
title: Linux-Permission-Capabilities
tags: OS
---

# Linux-Permission-Capabilities

Ran into a code today where I saw something like `/sbin/setcap cap_net_admin+p /usr/local/bin/{programname}`. I was like what is this `setcap`. Never heard of that. 

So, started with `man setcap` which spitted spitted `setcap - set file capabilities` !! Didn't really help.

`setcap` stands for set capabilities. Ever wondered how the permissions for a process is altered in Linux, enter `setcap`. For eg. `ping` utility requires to create raw packets. However, that requires elevated permission (aka to be run as `root`). That's not possible for all the users. So `ping` has been provided with `cap_new_raw` capability. You can confirm that through `getcap`.

```bash
vignesh@hostname:~$ sudo getcap /bin/ping
/bin/ping = cap_net_raw+ep`
```

So what I understand, `setcap` sets required privileges for a process without requiring to be run as a super user all the time.

Reference:

[getcap, setcap and file capabilities — insecure.ws](https://www.insecure.ws/linux/getcap_setcap.html)

[Working with Linux Capabilities](https://www.vultr.com/docs/working-with-linux-capabilities)

