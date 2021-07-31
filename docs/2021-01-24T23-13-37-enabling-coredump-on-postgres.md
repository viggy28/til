---
date: 2021-01-24T23:13:37-08:00
title: Enabling Coredump On Postgres
tags: OS,Postgres
---

# Enabling Coredump On Postgres

Noticed postgres backend process was SIGKILL. Figured it was OOM. However, while researching that I found about enabling coredump for Postgres

```bash
ulimit -c unlimited
```

Then use `gdb` to analyze the coredump generated.

Reference: https://wiki.postgresql.org/wiki/Getting_a_stack_trace_of_a_running_PostgreSQL_backend_on_Linux/BSD#Enabling_core_dumps

