---
date: 2021-03-06T01:02:42-08:00
title: New Timeout-Utility-Linux
tags: OS
---

# New Timeout-Utility-Linux

I was looking for a way to timeout a command in bash script. Stumbled into `timeout` command. 

Run a command with a time limit.

- Run `sleep 10` and terminate it, if it runs for more than 3 seconds:
    timeout 3s sleep 10

- Specify the signal to be sent to the command after the time limit expires. (By default, TERM is sent):
    timeout --signal INT 5s sleep 10