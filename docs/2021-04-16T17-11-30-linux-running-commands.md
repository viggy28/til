---
date: 2021-04-16T17:11:30-07:00
title: Linux Running Commands
tags: Linux
---

# Linux Running Commands

For a service to run a command it needs to have the permission. For eg. I was writing a program which needs to stop a systemd service. 

As an user myself who is part of the sudoers group I do

```bash
sudo systemctl stop main.service
```

where `main` is the service name. 

The key thing to note here is the command `sudo` in the front. 

How do i achieve the same using the program which I am writing?

Well, I can have the exact same command `sudo systemctl stop main.service` in my program however my program run as its own user `appuser`. So, now I need to add  `appuser` to sudoers group.

```bash
sudo -u otheruser executable
```

**sudoers file:**

```bash
%appuser ALL= NOPASSWD: /bin/systemctl stop main.service
```

**Reference:**

https://www.cyberciti.biz/open-source/command-line-hacks/linux-run-command-as-different-user/

https://unix.stackexchange.com/questions/192706/how-could-we-allow-non-root-users-to-control-a-systemd-service

