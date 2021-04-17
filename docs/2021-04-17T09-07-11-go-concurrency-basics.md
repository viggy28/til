---
date: 2021-04-17T09:07:11-07:00
title: Go-Concurrency-Basics
tags: Go,Concurrency
---

# Go-Concurrency-Basics

channels are like typed pipes, Where you can send something from one side and receive from other side. 

using channel direction operator <-. Depends on the direction of the variables around the operator, its either sending or receiving.

x := <-c // receive from c

c <- sum // send sum to c

fmt.Println("channel receive happening", <-c)

\> "By default, sends and receives block until the other side is ready. This allows goroutines to synchronize without explicit locks or condition variables"

This is something I didn't understand

One thing which I tested, if you try to receive more than what you sent in a channel, then it fails with 

```bash
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive]:
```

**Buffered chan:**

A channel with defined size is buffered channel. It blocks at the sender when the channel is full and at the receiver when the channel is empty

https://replit.com/@viggy28/concurrency#main.go

https://replit.com/@viggy28/concurrency-1#main.go

Also, didn't understand the select statement example https://tour.golang.org/concurrency/5

Channels are good for communication between goroutines. However, we don't need to communicate all the time, rather we just need to make sure one goroutine is accessing a variable at a time. That's where **mutex** helps.

Reference:

https://tour.golang.org/concurrency/9