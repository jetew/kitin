---
title: "使用 Caddy+Vmess+H2+TLS 搭建 V2ray"
slug: "caddy-v2ray"
date: 2022-10-06T10:08:09+08:00
lastmod: 2022-10-26T10:08:09+08:00
description: 基于 Caddy 服务，使用 Vmess+H2+TLS 配置 V2ray ，提升访问体验。
hidden: false
comments: true
draft: false
toc: true
enableTocContent: true
weight: false
tags:
- caddy
- v2ray
categories:
- 学习
image: /images/caddy-v2ray/0.png
---

{{< img src="/images/caddy-v2ray/0.png" caption="◎ Project V" position="center" >}}

## 前言

由于从 Typecho 更换到了 Hugo ，最近将服务器的 Nginx 服务更换成了 Caddy，便折腾起了 Caddy+V2ray 的组合；而这一次准备将之前的 Websocket 更换成 H2 传输；所以在这里进行一个记录，方便以后进行查阅，搭建过程也可以参考之前这篇 [《基于 Nginx 的 Vmess+WS+TLS 搭建 V2ray》](/p/nginx-v2ray)

### Caddy 介绍

Caddy 服务器（或称Caddy Web）是一个开源的，使用 Golang 编写，支持 HTTP/2 的 Web 服务端，它使用 Golang 标准库提供 HTTP 功能。Caddy 一个显著的特性是默认启用 HTTPS，它是第一个无需额外配置即可提供 HTTPS 特性的 Web 服务器。Caddy 可以提供各种网站技术，它也可以作为反向代理和负载均衡器。Caddy 的大部分功能都以中间件的形式实现，并通过 Caddyfile 中的指令（用于配置 Caddy 的文本文件）进行控制。

> 简单来说就是配置简单，开箱即用！

<!--more-->

---

## 架设教程

### 准备工作

首先对服务器进行初步部署，参考[这里](/p/setvps/)；并将域名提前解析至你的服务器。

### 安装配置 Caddy

#### 安装 Caddy

- <a href="https://caddyserver.com" target="_blank">Caddyserver 官网</a>

进入 Caddyserver 官网，直接下载源文件，或者使用命令行进行安装：

```bash
apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list
apt update
apt install caddy
```

安装完成后，输入 `which caddy` 或者 `caddy version` 查看是否安装成功，安装完成后，打开浏览器输入服务器 IP 地址，便可看到默认的欢迎页面。

---

#### 配置 Caddy

使用 `vim` 命令，或者编辑器对配置文件进行编辑，路径为： **/etc/caddy/Caddyfile**

配置文件如下：

```caddyfile
# 域名
xx.com {
    # 设置网站根目录
    root * /var/www/xx.com
    # /path 路径，端口与 V2ray 保持一致
    reverse_proxy /path 127.0.0.1:10086 {
        transport http {
            versions h2c
        }
    }
    # TLS 设置
    tls xx@email.com
}
```

配置完成后输入以下命令检查配置：

```bash
caddy adapt --config /etc/caddy/Caddyfile
```

如未报错，则表示配置无误

重载 caddyfile：

```bash
caddy reload --config /etc/caddy/Caddyfile
```

---

### 安装配置 V2ray

#### 安装 V2ray

安装 V2ray，选择官方的脚本：

```diff
- bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --version 4.34.0
+ bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

~~由于最近版本问题，添加 `--version 4.34.0`` 指定安装 4.34.0 版本~~

>  版本问题已解决，可以直接使用新版本安装 (2022-10-26 更新)

---

#### 配置 V2ray

##### 获取 UUID

键入下面命令，获取一个随机 UUID：

```diff
- /usr/local/bin/v2ctl uuid
+ /usr/local/bin/v2ray uuid
```
鼠标左键选中生成的 UUID 点击右键即可复制

> 由于版本更新，`v2ctl` 已整合到 `v2ray` 中 (2022-10-26 更新)

---

##### 配置文件

使用 `vim` 命令或者其他编辑器进行编辑，文件路径为 **/usr/local/etc/v2ray/config.json** 按照下面给出的标注位置修改即可，修改完成后删除标注

```json
{
  "log": {
    "loglevel": "warning",
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log"
  },
  "inbounds": [
    {
      "port": 10086, // 端口，可自行设置；与 Caddyfile 保持一致
      "listen": "127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "" // 刚才生产的 UUID 粘贴到这
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "security": "none",
        "network": "h2",
        "httpSettings": {
          "path": "/path", // path 路径可自行设置，与 Caddyfile 保持一致
          "host": [
            "xx.com" // 域名
          ]
        }
      }
    }
  ],
  "outbounds": [
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag": "blocked",
      "protocol": "blackhole",
      "settings": {}
    }
  ],
  "routing": {
    "domainStrategy": "AsIs",
    "rules": [
      {
        "type": "field",
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "blocked"
      }
    ]
  },
  "dns": {
    "servers": [
        "https+local://1.1.1.1/dns-query",
        "1.1.1.1",
        "1.0.0.1",
        "8.8.8.8",
        "8.8.4.4",
        "localhost"
    ]
  }
}
```

---

### 启动服务

输入以下命令，重新启动服务并添加开机自启：

```bash
systemctl restart v2ray
systemctl restart caddy
systemctl enable v2ray
systemctl enable caddy
```

## 总结

以上就是基于 Caddy 的 V2ray 搭建过程了，包括前面一篇基于 Nginx 的也是一样；可以直接拿来搭建。网上也有很多类似的教程，但是很多都是基于 Caddy V1 的配置，也比较乱；这些都是我自己长时间使用以来一直在用的配置，至于传输方式和协议什么的，自己可以根据实际情况选择。由于某些不可抗因素，参考资料什么的就不贴上了。