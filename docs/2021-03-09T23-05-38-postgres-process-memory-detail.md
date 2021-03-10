---
date: 2021-03-09T23:05:38-08:00
title: Postgres-Process-Memory-Detail
tags: Postgres
---

# Postgres-Process-Memory-Detail

To understand the memory consumption of a Postgres process one can use gdb.

```bash
gdb -p $the_backend_pid
(gdb) p MemoryContextStats(TopMemoryContext)
```

In the postgres log:

```bash
TopMemoryContext: 8192 total in 1 blocks; 4632 free (0 chunks); 3560 used
  LOCALLOCK hash: 8192 total in 1 blocks; 552 free (0 chunks); 7640 used
  Timezones: 104120 total in 2 blocks; 2616 free (0 chunks); 101504 used
  Postmaster: 24576 total in 2 blocks; 23712 free (14 chunks); 864 used
    ident parser context: 1024 total in 1 blocks; 760 free (0 chunks); 264 used
    hba parser context: 9216 total in 4 blocks; 3280 free (6 chunks); 5936 used
  ErrorContext: 8192 total in 1 blocks; 7928 free (3 chunks); 264 used
Grand total: 163512 bytes in 12 blocks; 43480 free (23 chunks); 120032 used
```

Ref:

1. https://wiki.postgresql.org/wiki/Developer_FAQ#Examining_backend_memory_use
2. https://dba.stackexchange.com/questions/160887/how-can-i-find-the-source-of-postgresql-per-connection-memory-leaks/222815#222815



