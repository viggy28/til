---
date: 2021-03-16T22:01:30-07:00
title: Difference-Between-COPY-And-\COPY
tags: postgres
---

# Difference-Between-COPY-And-\COPY

I came across `COPY` and `\COPY` command in postgres. Didn't really understand what's the difference between two and when to use which one.

From the [postgresql wiki](https://wiki.postgresql.org/wiki/COPY) itself

```
COPY is the Postgres method of data-loading. Postgres's COPY comes in two separate variants, COPY and \COPY: COPY is server based, \COPY is client based.

COPY will be run by the PostgreSQL backend (user "postgres"). The backend user requires permissions to read & write to the data file in order to copy from/to it. You need to use an absolute pathname with COPY. \COPY on the other hand, runs under the current $USER, and with that users environment. And \COPY can handle relative pathnames. The psql \COPY is accordingly much easier to use if it handles what you need.
```

\COPY -> psql command; reads and writes output on the client server.

COPY -> Postgres command that can be run from any postgres client (i.e. from tools like dbeaver, pgadmin etc.); reads and writes output on the server where postgres is running.

This is a good explanation: https://stackoverflow.com/questions/51681905/difference-between-copy-and-copy-commands-in-postgresql

