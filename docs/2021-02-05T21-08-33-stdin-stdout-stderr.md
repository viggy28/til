---
date: 2021-02-05T21:08:33-08:00
title: Stdin-Stdout-Stderr
tags: OS,OpenSSL
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

**Update1:**

I was using OpenSSL command and I was always getting some messages no matter how I grep.

```
vigneshravichandran@C02YT0F1LVDQ til % openssl s_client -connect viggy28.dev:443 |  openssl x509 -text |grep "After"
depth=2 C = IE, O = Baltimore, OU = CyberTrust, CN = Baltimore CyberTrust Root
verify return:1
depth=1 C = US, O = "Cloudflare, Inc.", CN = Cloudflare Inc ECC CA-3
verify return:1
depth=0 C = US, ST = California, L = San Francisco, O = "Cloudflare, Inc.", CN = sni.cloudflaressl.com
verify return:1
            Not After : Jun 10 23:59:59 2022 GMT
^C
vigneshravichandran@C02YT0F1LVDQ til %
```

Even though I grep `After` we still some output at the begging. Later I found in [this](https://stackoverflow.com/questions/39385795/suppress-openssl-output-during-grep), that output is not from stdout its from stderr. That made me wondering how can I figure out whether an output is from stdout or stderr once its printed in the terminal. Because there is no way I can differentiate by looking at the output. As per [this](https://unix.stackexchange.com/questions/186449/how-to-tell-if-output-of-a-command-or-shell-script-is-stdout-or-stderr) there is no way I can find once its printed on the terminal.

To suppress that stderr message I can use `2>/dev/null`
```
vigneshravichandran@C02YT0F1LVDQ til % openssl s_client -connect viggy28.dev:443 2>/dev/null |  openssl x509 -text |grep "After"
            Not After : Jun 10 23:59:59 2022 GMT
^C
vigneshravichandran@C02YT0F1LVDQ til %
```

References: https://www.howtogeek.com/435903/what-are-stdin-stdout-and-stderr-on-linux/
https://stackoverflow.com/questions/39385795/suppress-openssl-output-during-grep
https://unix.stackexchange.com/questions/186449/how-to-tell-if-output-of-a-command-or-shell-script-is-stdout-or-stderr





