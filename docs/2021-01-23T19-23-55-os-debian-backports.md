---
date: 2021-01-23T19:23:55-08:00
title: OS Debian Backports
tags: OS
---

# OS Debian Backports

when I checked for `s3cmd` package in buster and I couldn't find it. Checked here https://packages.debian.org/search?keywords=s3cmd and noticed backport. Need to add a source for backport in apt.d/ directory and able to install.

```
Backports are packages taken from the next Debian release (called "testing"), adjusted and recompiled for usage on Debian stable. Because the package is also present in the next Debian release, you can easily upgrade your stable+backports system once the next Debian release comes out. (In a few cases, usually for security updates, backports are also created from the Debian unstable distribution.)
```



`