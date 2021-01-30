---
date: 2021-01-29T20:27:11-08:00
title: Bash-Scripting-Gotchas
tags: OS
---

# Bash-Scripting-Gotchas

-  `#! ` shebang on the top of the line is a special character that conveys what's the shell that needs to be used. for eg. `#! /bin/bash` conveys run the rest of the script using `bash` shell

- There are two ways to run a bash script.
  - Using a stand alone program (process)
    - It gets its own PID, Shell.
    - If you need to pass any variables that needs to be exported do`export VARNAME=blah` to use VARNAME in your script.
  - Using the existing process by the current shell
    - Done by `source script.sh` or `. script.sh`
    - since its same shell, you can use variable set in your current shell `VARNAME=blah` in the script
  - Ref: https://stackoverflow.com/questions/14744904/how-to-execute-script-in-the-current-shell-on-linux
- If you want your script to exit at the failure of any command then add `set -e`. To know whether a command failed or not, run echo $? after the execution. For eg. `grep` command on a string which doesn't exist doesn't fail. However if you check echo $? it's value is 1.
- Ref for `set -e` gotchas http://mywiki.wooledge.org/BashFAQ/105
  - commands that involves `|` need `set -o pipefail`
- functions can't return string. https://stackoverflow.com/questions/24676564/bash-return-a-string-from-bash-function
- To see execution of each line do `bash -x script.sh`

