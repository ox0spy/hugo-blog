---
title: "gradle代理设置"
date: 2017-12-15T19:22:00+08:00
tags: ["gradle"]
author: "ox0spy"
categories: ["android"]
---

Android使用Gradle做构建工具，编译apk时可能需要从网上下载一些依赖包，国内网络情况大家都懂得。

## Gradle代理设置

    :::bash
    $ cat ~/.gradle/gradle.properties
    systemProp.http.proxyHost=your-proxy
    systemProp.http.proxyPort=proxy-port
    #systemProp.http.proxyUser=userid
    #systemProp.http.proxyPassword=password
    #systemProp.http.nonProxyHosts=*.nonproxyrepos.com|localhost