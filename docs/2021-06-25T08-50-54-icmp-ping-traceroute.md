---
date: 2021-06-25T08:50:54-07:00
title: ICMP-ping-traceroute
tags: Networking,AWS
---

# Icmp-Ping-Traceroute
I was trying to `ping` my aws server and it was unreachable. It's security group issue. Added an Custom ICMP rule and it started working. Ref1: https://intellipaat.com/community/41740/can-i-ping-an-ec2-instance-from-my-local-machine#:~:text=You%20can%20ping%20it%20from,inbound%20rules%20and%20click%20Edit.&text=For%20source%2C%20add%20Anywhere%20so,from%20any%20machine%20you%20want.


So, what is `ICMP`? 
Internet Control Message Protocol. Hmm. Okay, that seems like another protocol similar to TCP and UDP?

Well, ICMP is not a tranport layer protocol. That means no connections, no handshakes etc.

> The primary purpose of ICMP is for error reporting. When two devices connect over the Internet, the ICMP generates errors to share with the sending device in the event that any of the data did not get to its intended destination. 

> A secondary use of ICMP protocol is to perform network diagnostics; the commonly used terminal utilities traceroute and ping both operate using ICMP.

**ICMP is the protocol behind `ping` and `traceroute` utility**

Ref2: https://www.cloudflare.com/learning/ddos/glossary/internet-control-message-protocol-icmp/


