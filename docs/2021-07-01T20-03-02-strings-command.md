---
date: 2021-07-01T20:03:02-07:00
title: strings-Command
tags: Linux
---

# Strings-Command

Today I came across `strings` command. It helps to find commands in an executable or object file.


```
pi@raspberrypi:~ $ strings argo-tunnel/cloudflared |grep "tunnel" | head
tunnels
tunnelID
	tunnelsHA
*tunnel.Info
tunnelActive
tunnelErrors
tunnelHostText
tunnelEventChan
tunnelsConnecting
tunnelstoreClient
```

Provided by `binutils` package

```
pi@raspberrypi:~ $ sudo dpkg -S /usr/bin/strings
binutils: /usr/bin/strings
pi@raspberrypi:~ $
```

