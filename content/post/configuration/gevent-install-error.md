---
title: "python gevent安装报错"
date: 2017-12-15T19:21:58+08:00
tags: ["python", " pip", " gevent"]
author: "ox0spy"
categories: ["configuration"]
---

Mac上pip安装gevent报错信息如下：

    libev/ev.c:1029:42: error: '_Noreturn' keyword must precede function declarator

解决：

    :::bash
    $ CFLAGS='-std=c99' pip install gevent

这里有讨论：https://github.com/NixOS/nixpkgs/issues/8569