---
date: 2021-02-05T21:08:33-08:00
title: Stdin-Stdout-Stderr
tags: OS
---

# Stdin-Stdout-Stderr

I have across this many times and used it in programs, but never played with it.

Basically these are standard streams. Think of streams as pipes. Instead of carrying water, they carry data generally to/from the terminal to/from the program.

These values are always used for `stdin`, `stdout,` and `stderr`:

- *0*: stdin
- *1*: stdout
- *2*: stderr

The order is important. Say you use all three streams in a same command (or process ) and if the stdin fails it doesn't respect what we mention wrt to stdout or stderr.

```
vigneshravichandran@C02YT0F1LVDQ ~ % read var1 1>stdoutfile 2>stderrfile 0<stdinfile
zsh: no such file or directory: stdinfile
```

In the above comand error didn't get redirected to stderrfile because 0 (stdin) operation failed

```
vigneshravichandran@C02YT0F1LVDQ ~ % echo "hi" 1>/bin/stdoutfile 2>stderrfile
zsh: operation not permitted: /bin/stdoutfile
```

Same as above. stdout takes higher priority than stderr. So error is still displayed instead of redirecting

```
vigneshravichandran@C02YT0F1LVDQ ~ % echo "hi" 1>stdoutfile 2>stderrfile
vigneshravichandran@C02YT0F1LVDQ ~ % cat stdoutfile && cat stderrfile
hi
```

Finally, things working :) 

References: https://www.howtogeek.com/435903/what-are-stdin-stdout-and-stderr-on-linux/



