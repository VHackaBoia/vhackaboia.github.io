---
layout: default
ctf: "b01lers 2023"
name: "cheating scandal"
description: "Cheating OSINT"
date: 2023-03-18
category: "Osint"
points: 370
solves: 18
tags: ["docker", "docker-hub", "discord", "discord-bot", "privilage escalation"]
author: ["paololo", "rox", "quby"]
---

# cheating scandal (370 pts, 18 solved)


Purdue Pete confessed that he cheated on his cs homework. He said He doesnt know how he got the cheat code. All that Pete remembers is a weird file with FROM experienceg48/cheat:latest at the top. Can you help us get the contact information of the perpetrator?

Authors: CygnusX, Bilbin, Deltaforce, and Colbyjack

**Hint! Note: the linked-in link is not intended to be part of this chal, and was left there by mistake. Please disregard it**

## Discovery Phase

Description give us a docker image (`experienceg48/cheat:latest`), so we can try to run it.

**NOTE: image is an arm64 image, so you need to run it on a arm64 machine (MacBook M1 is perfect)**


```python
!docker run -it experienceg48/cheat:latest /bin/bash
```

### Docker Exploration

Explore code folder:

```
.
├── Dockerfile
├── cheatscript.py
├── requirements.txt
└── test.Dockerfile
```

Cheatscript.py:

```python
#whatroot
```

### Docker Hub

Nothing interesting here, docker image is probably not useful. Let's try to find the docker image on docker hub.

We found the docker image on docker hub: https://hub.docker.com/r/experienceg48/cheat

We can see that the image docker has different tags:

- latest
- v1
- v2

#### Docker Image v2

Let's explore the v2 image tag:

```
.
├── Dockerfile
├── No-Cheating-Here
│   ├── .git
│   │   ├── HEAD
│   │   ├── branches
│   │   ├── config
│   │   ├── description
│   │   ├── hooks
│   │   │   ├── applypatch-msg.sample
│   │   │   ├── commit-msg.sample
│   │   │   ├── fsmonitor-watchman.sample
│   │   │   ├── post-update.sample
│   │   │   ├── pre-applypatch.sample
│   │   │   ├── pre-commit.sample
│   │   │   ├── pre-merge-commit.sample
│   │   │   ├── pre-push.sample
│   │   │   ├── pre-rebase.sample
│   │   │   ├── pre-receive.sample
│   │   │   ├── prepare-commit-msg.sample
│   │   │   ├── push-to-checkout.sample
│   │   │   └── update.sample
│   │   ├── index
│   │   ├── info
│   │   │   └── exclude
│   │   ├── logs
│   │   │   ├── HEAD
│   │   │   └── refs
│   │   │       ├── heads
│   │   │       │   └── main
│   │   │       └── remotes
│   │   │           └── origin
│   │   │               └── HEAD
│   │   ├── objects
│   │   │   ├── info
│   │   │   └── pack
│   │   │       ├── pack-d8c436846719a95efef6cd7ef71cb73464740f13.idx
│   │   │       └── pack-d8c436846719a95efef6cd7ef71cb73464740f13.pack
│   │   ├── packed-refs
│   │   └── refs
│   │       ├── heads
│   │       │   └── main
│   │       ├── remotes
│   │       │   └── origin
│   │       │       └── HEAD
│   │       └── tags
│   └── andrewtate
│       └── my_thoughts.txt
├── cheatscript.py
├── requirements.txt
└── test.Dockerfile
```

A git repository is in the docker image, let's explore it.


```python
!docker run -it experienceg48/cheat:v2 /bin/bash -c "cat /code/No-Cheating-Here/.git/config"
```

    [core]
    	repositoryformatversion = 0
    	filemode = true
    	bare = false
    	logallrefupdates = true
    [remote "origin"]
    	url = https://github.com/zacianstorm/No-Cheating-Here.git
    	fetch = +refs/heads/*:refs/remotes/origin/*
    [branch "main"]
    	remote = origin
    	merge = refs/heads/main


#### Github Repository

A github repository is found: [https://github.com/zacianstorm/No-Cheating-Here](https://github.com/zacianstorm/No-Cheating-Here)

Exploring the repository, we can see some stuff about cheating and andrew tate. Let's give a look to the commit history.

We can see that in the commit history, owner deleted a pic ```andrewtate/andrew_tate.jpg```. Downloading the file, we can see that it's a picture of Andrew Tate. Let's try to find more information using exiftool.



```python
!exiftool "./cheating scandal/andrew_tate.jpg"
```

    ExifTool Version Number         : 12.50
    File Name                       : andrew_tate.jpg
    Directory                       : ./cheating scandal
    File Size                       : 17 kB
    File Modification Date/Time     : 2023:03:20 14:43:35+01:00
    File Access Date/Time           : 2023:03:20 14:43:36+01:00
    File Inode Change Date/Time     : 2023:03:20 14:43:35+01:00
    File Permissions                : -rw-r--r--
    File Type                       : JPEG
    File Type Extension             : jpg
    MIME Type                       : image/jpeg
    JFIF Version                    : 1.01
    Resolution Unit                 : inches
    X Resolution                    : 96
    Y Resolution                    : 96
    Exif Byte Order                 : Big-endian (Motorola, MM)
    Artist                          : https://twitter.com/WilhelmSexerton
    XP Author                       : https://www.linkedin.com/in/wilhelm-sexerton-bb0799265/
    Padding                         : (Binary data 2060 bytes, use -b option to extract)
    Current IPTC Digest             : 7f3300ba5965df31df76885f3681f7f0
    Coded Character Set             : UTF8
    Envelope Record Version         : 4
    By-line                         : https://twitter.com/WilhelmSexer
    Application Record Version      : 4
    XMP Toolkit                     : Image::ExifTool 12.55
    About                           : uuid:faf5bdd5-ba3d-11da-ad31-d33d75182f1b
    Creator                         : https://twitter.com/WilhelmSexerton
    Image Creator Name              : https://twitter.com/WilhelmSexerton
    Image Width                     : 279
    Image Height                    : 181
    Encoding Process                : Baseline DCT, Huffman coding
    Bits Per Sample                 : 8
    Color Components                : 3
    Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
    Image Size                      : 279x181
    Megapixels                      : 0.050


#### Twitter Account

We ignore linkedin account because it's not useful (hint).

Finally we have found cheater's twitter account: [https://twitter.com/WilhelmSexerton](https://twitter.com/WilhelmSexerton)

Unfortunately, nothing interesting in his posts. Nevertheless, we found a twitter list with some information ([https://twitter.com/i/lists/1632902931304390656](https://twitter.com/i/lists/1632902931304390656)):

- List Name: `Hammer and chisel`
- List Description: `zcJJjuXJ2h`

Let's try to find more information about this list. We tried to google list name: `Hammer and chisel` is the original name of Discord. So we infered that the list description is a discord invite link.

#### Discord Server


We need to use admin command `\contact` in order to get flag, but we need to have Admin role.

Bot can be easily tricked inviting it to our private channel (in which we have Admin role) and then using `\contact` command.

An usefull guide can be find here : [https://scc-luhack.lancs.ac.uk/writeups/view/htbxuni-ai](https://scc-luhack.lancs.ac.uk/writeups/view/htbxuni-ai)