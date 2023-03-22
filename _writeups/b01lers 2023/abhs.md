---
layout: default
ctf: "b01lers 2023"
name: "abhs"
description: "Just a warmup."
date: 2023-03-18
category: "Misc"
points: 79
solves: 92
tags: ["shell"]
author: ["quby"]
---
# abhs (79 pts, 92 solved) 

Just a warmup.

```nc abhs.bctf23-codelab.kctf.cloud 1337```

Author: dm

# Solution

### Discovering Phase

Try to connect to the server:


```bash
!echo "ls" | nc abhs.bctf23-codelab.kctf.cloud 1337
```

    == proof-of-work: disabled ==
    == A Bonkers Homemade Shell ==
    $ chal.py
    flag.txt
    wrapper.sh
    $ Traceback (most recent call last):
      File "chal.py", line 10, in <module>
        l = input("$ ")
    EOFError: EOF when reading a line


It looks easy, we have a shell, let's try to get the flag:


```bash
!echo "cat flag.txt" | nc abhs.bctf23-codelab.kctf.cloud 1337
```

    == proof-of-work: disabled ==
    == A Bonkers Homemade Shell ==
    $ sh: 1: act: not found
    $ Traceback (most recent call last):
      File "chal.py", line 10, in <module>
        l = input("$ ")
    EOFError: EOF when reading a line


Something goes wrong, let's try another command for reading the flag:


```bash
!echo "more flag.txt" | nc abhs.bctf23-codelab.kctf.cloud 1337
```

    == proof-of-work: disabled ==
    == A Bonkers Homemade Shell ==
    $ sh: 1: emor: not found
    $ Traceback (most recent call last):
      File "chal.py", line 10, in <module>
        l = input("$ ")
    EOFError: EOF when reading a line


Wut? I didn't ever used emor command, wait a minute...

Command is ordered alphabetically, ```emor``` instead of ```more```.

### Exploiting Phase

We need to find a command for read file ordered alphabetically, in order to get allowable command list:

```bash
!bash -c "compgen -c | grep -ix 'a*b*c*d*e*f*g*h*i*j*k*l*m*n*o*p*q*r*s*t*u*v*w*x*y*z*'"
```

    fi
    for
    do
    in
    bg
    cd
    dirs
    fg
    ttx
    ttx
    gmv
    pp
    gpr
    go
    ptx
    gs
    dot
    ttx
    R
    gln
    gls
    xz
    pt
    ahost
    nop
    ht
    gsx
    gtty
    gptx
    nns
    jq
    ip
    npx
    gstty
    gio
    uux
    cmp
    pp
    tty
    du
    host
    jot
    bc
    fmt
    cu
    ex
    lp
    jps
    delv
    lpq
    rs
    env
    pr
    su
    cpp
    apt
    ar
    jjs
    bg
    aa
    at
    as
    fg
    cd
    cc
    lpr
    git
    w
    df
    stty
    ps
    dd
    mv
    ln
    ls
    cp
    amt
    ac
    gpt
    ab
    ttx
    gmv
    pp
    gpr
    go
    ptx
    gs
    dot
    ttx
    R
    gln
    gls
    xz
    pt
    ahost
    nop
    ht
    gsx
    gtty
    gptx
    nns
    jq
    ip
    npx
    gstty
    gio
    uux
    cmp
    pp
    tty
    du
    host
    jot
    bc
    fmt
    cu
    ex
    lp
    jps
    delv
    lpq
    rs
    env
    pr
    su
    cpp
    apt
    ar
    jjs
    bg
    aa
    at
    as
    fg
    cd
    cc
    lpr
    git
    w
    df
    stty
    ps
    dd
    mv
    ln
    ls
    cp
    amt
    ac
    gpt
    ab


After reading man page of a lot of commands, we found ```pr```:

```pr - convert text files for printing```

Finally, let's try to get the flag:


```bash
!echo "pr flag.txt" | nc abhs.bctf23-codelab.kctf.cloud 1337
```

    == proof-of-work: disabled ==
    == A Bonkers Homemade Shell ==
    $ pr: .afglttx: No such file or directory
    $ Traceback (most recent call last):
      File "chal.py", line 10, in <module>
        l = input("$ ")
    EOFError: EOF when reading a line


flag.txt is ordered alphabetically too...

### Solution

In order to get flag we can use regex to get all files of the current directory:


```bash
!echo "pr *" | nc abhs.bctf23-codelab.kctf.cloud 1337 | grep "bctf"
```

    #bctf{gr34t_I_gu3ss_you_g0t_that_5orted_out:P}

