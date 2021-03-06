---
title: "nginx配置ssl证书"
date: 2017-12-15T19:21:58+08:00
tags: ["nginx", " ssl", " https"]
author: "ox0spy"
categories: ["configuration"]
---

本文参考:

* <http://www.cocoachina.com/ios/20150703/12392.html>
* <https://hack0nair.me/2014-08-14-nginx-with-ssl-of-wosign/>
* <http://note.sdo.com/u/634668972018478681/n/Z1NG3C6A959F29DD2FA2D543B8C9B30B551810C3>
* <http://op.baidu.com/2015/04/https-s01a01/>
* <http://oncenote.com/2014/10/21/Security-1-HTTPS/>
* <http://oncenote.com/2015/09/16/Security-2-HTTPS2/>
* <https://www.vobe.io/114>


# Nginx配置SSL证书, 启用HTTPS

> iOS9把所有的http请求都改为https了：iOS9系统发送的网络请求将统一使用TLS 1.2 SSL。
> 采用TLS 1.2 协议，目的是 强制增强数据访问安全，而且 系统 Foundation 框架下的相关网络请求，
> 将不再默认使用 Http 等不安全的网络协议，而默认采用 TLS 1.2。服务器因此需要更新，以解析相关数据。
> 如不更新，可通过在 Info.plist 中声明，倒退回不安全的网络请求。

为了客户端接口更加安全，申请免费SSL证书，启用HTTPS。

## 申请证书

从Wosign申请免费证书，[地址](https://www.wosign.com/DVSSL/DV_KuaiSSL_Free.htm)

## 配置Nginx

证书申请到会提示下载一个zip文件，解压缩后有 `for Nginx.zip`，解压缩`for Nginx.zip`得到 `*.crt` 和 `*.key`。
将`.crt` 和 `.key` 文件放入Nginx主机的 /etc/ssl 目录。

生成dhparam.pem:

    :::bash
    $ cd /etc/ssl/certs
    $ openssl dhparam -out dhparam.pem 4096

同时，启用了spdy，参考: <https://www.vobe.io/114>

Nginx配置文件(`/etc/nginx/sites-avaiable/default`)如下:

    #server {
    #   listen 80;
    #   server_name your-domain;
    #   add_header Strict-Transport-Security max-age=15768000;
    #   return 301 https://your-domain$request_uri;
    #}

    server {
        listen 80; # 如果希望http直接跳转到https，删除该行，并去掉上面的注释
        listen 443 ssl spdy;
        spdy_headers_comp 1;
        server_name your-domain;

        ssl on;
        ssl_certificate      /etc/ssl/your-domain.crt;
        ssl_certificate_key  /etc/ssl/your-domain.key;

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

        ssl_ciphers 'AES256+EECDH:AES256+EDH:!aNULL';
        #ssl_ciphers "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4"

        ssl_prefer_server_ciphers  on;
        ssl_dhparam /etc/ssl/certs/dhparam.pem;

        ssl_session_cache    shared:SSL:20m;
        ssl_session_timeout  60m;

        ssl_stapling on;
        ssl_stapling_verify on;
        #ssl_trusted_certificate /etc/nginx/cert/trustchain.crt;
        #resolver 8.8.4.4 8.8.8.8 valid=300s;
        #resolver_timeout 10s;

        add_header Strict-Transport-Security max-age=15768000;

        add_header X-Frame-Options DENY;
        add_header X-Content-Type-Options nosniff;

        add_header Alternate-Protocol 443:npn-spdy/3.1;

        proxy_set_header   Host                 $http_host;
        proxy_set_header   X-Forwarded-Proto    $scheme;
        proxy_set_header   X-Forwarded-For      $remote_addr;
        proxy_redirect     off;

        location /your-service {
            proxy_pass         http://localhost:8888;
        }
    }

## 测试HTTPS

curl测试服务器上的https接口:

    :::bash
    $ curl https://your-domain/your-services

测试HTTPS配置的是否安全：[https://www.ssllabs.com/ssltest/](https://www.ssllabs.com/ssltest/)

注：如果安全评分比较低，请认真阅读[Strong SSL Security on nginx](http://note.sdo.com/u/634668972018478681/n/Z1NG3C6A959F29DD2FA2D543B8C9B30B551810C3)

## 测试SPDY

检测配置的SPDY：<https://spdycheck.org/>