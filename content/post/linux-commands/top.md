---
title: "top"
date: 2017-12-15T19:21:57+08:00
tags: ["linux", " command", " top"]
author: "crazygit"
categories: ["linux-commands"]
---

## 简介

top命令的具体就不解释了，记录一些实用的命令

```bash

# redis用户此时所使用的内存总数
# 这里的内存使用
top -bn1 -u redis |  awk '{mem += $10} END {print mem}'

# 查看服务器所有进程的内容使用总数
# 结果可能大于100%，反应出目前机器内容可用较少, 具体解释可以参考
# http://serverfault.com/questions/471757/sum-of-memory-of-few-processes-in-top-is-greater-than-100
$ top -bn1 |  awk '{mem += $10} END {print mem}'
135.6


```