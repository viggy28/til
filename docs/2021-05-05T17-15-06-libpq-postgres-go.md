---
date: 2021-05-05T17:15:06-07:00
title: Libpq-Postgres-Go
tags: Libpq,Go,Postgres,C
---

# Libpq-Postgres-Go

After I started working on Postgres, I have heard this term `libpq` enough times, but never had a good grasp of it. After digging around this topic for a couple of days, here is my understanding.

From the Postgres doc https://www.postgresql.org/docs/9.5/libpq.html, 

> libpq is the C application programmer's interface to PostgreSQL. libpq is a set of library functions that allow client programs to pass queries to the PostgreSQL backend server and to receive the results of these queries

1. Okay, its an API to Postgres written in C
2. Where is its source code? Its actually in the same repo as Postgres https://github.com/postgres/postgres/tree/master/src/backend/libpq
3. client programs like `psql` , `pg_basebackup` etc.. to connect to Postgres

> libpq is also the underlying engine for several other PostgreSQL application interfaces, including those written for C++, Perl, Python, Tcl and ECPG.

I checked python, it looks like [psycopg2](https://pypi.org/project/psycopg2/)  is a popular package that's recommended to connect to Postgres. From the earlier link

> Psycopg 2 is mostly implemented in C as a libpq wrapper,

Going back to libpq documentation,

> Client programs that use libpq must include the header file `libpq-fe.h` and must link with the libpq library.

Source of the header file: https://github.com/postgres/postgres/blob/master/src/interfaces/libpq/libpq-fe.h

Its been a long time since I dealt with C, so I needed a primer on what is header file. @flaviocopes has a good article on this https://flaviocopes.com/c-header-files/

Here is an example of how to include this header file in a C program

```c
#include <stdio.h>
#include <libpq-fe.h>

int main() {
    
    int lib_ver = PQlibVersion();

    printf("Version of libpq: %d\n", lib_ver);
    
    return 0;
}
```

Ref: https://zetcode.com/db/postgresqlc/

So far so good, but if you try to run this program likely you will get an error that header file doesn't exist.

Enter [libpq-dev](https://packages.debian.org/buster/libpq-dev) - A debian package that provides header files. You can see what are the files that this package contains https://packages.debian.org/stretch/amd64/libpq-dev/filelist

At the same time, I stumbled upon another debian package [libpq5](https://packages.debian.org/stretch/libpq5) wondering what does that does compared to libpq-dev. That got cleared when I installed [postgresql-client](https://packages.debian.org/buster/postgresql-client). It requires libpq5. So, yeah libpq5 is the C API (libpq) that we saw very first of this note. Don't know why its been named as libpq5 and why not just libpq. libpq-dev is the development package required if you are writing your own client program (like the above example C program) to connect to Postgres.

I am mostly writing code in Go these days and while connecting to Postgres I use the package [lib/pq](https://github.com/lib/pq) along with the standard [sql](https://github.com/golang/go/tree/master/src/database/sql) package. I wondered how's the `lib/pq` package connects to Postgres. It doesn't 

1. Have any C `libpq` related library 
2. It doesn't make any system related calls.

There is another package [go-libpq](https://github.com/jgallagher/go-libpq), which `cgo`. Basically, it calls the actual libpq functions.

```go
db := C.PQconnectdb(params)
	if C.PQstatus(db) != C.CONNECTION_OK {
		defer C.PQfinish(db)
		return nil, errors.New("libpq: connection error " + C.GoString(C.PQerrorMessage(db)))
	}
```

Also, it imports the `libpq-fe.h` . Remember the header file that we saw in another example

```go
package libpq

/*
#include <stdlib.h>
#include <libpq-fe.h>
*/
import "C"
```

I posted the question in a forum and I have been told 

>The PostgreSQL wire protocol is openly documented (and not very complex). It can be implemented in an arbitrary language, there is no force to use the "official" C library.
>As for system calls, a database driver typically needs only to open a TCP connection, read and write from/to it, and close it when work is finished. In Go, this usually done with the Dial() function of the "net" package. The returned type (Conn) has Read(), Write() and Close() methods. There is no need to make direct system calls from a database driver.

That made sense, since I did see the Dial() functions and TCP calls in the `lib/pq` package. 

Did a G-search and found the wire protocol documentation https://www.postgresql.org/docs/current/protocol.html. As a continuation, I would like to understand the protocol and implement a simple version of this driver (just for fun) one day.



