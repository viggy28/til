---
date: 2021-03-06T01:29:30-08:00
title: New Systemd-Quirk-Stolon
tags: OS Postgres
---

# New Systemd-Quirk-Stolon

When doing stolon restart using systemd without KillMode setting was noticing `smart shutdown request` followed by `fast shutdown` . Adding `KillMode` config to the systemd unit fixed that problem.