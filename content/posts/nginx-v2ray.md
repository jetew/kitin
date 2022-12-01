---
title: "基于 Nginx 的 Vmess+WS+TLS 搭建 V2ray"
slug: "nginx-v2ray"
date: 2022-03-01T17:14:41+08:00
lastmod: 2022-10-26T10:50:47+08:00
description: 基于 Nginx 服务，用 Vmess 协议 +WS+TLS 配置 V2ray ，提升访问体验。
hidden: false
comments: true
draft: false
toc: true
enableTocContent: true
weight: false
tags:
- lnmp
- v2ray
- nginx
categories:
- 学习
image: /images/nginx-v2ray/0.png
---

{{< img src="/images/nginx-v2ray/0.png" caption="◎ Project V" position="center" >}}

## 前言

最近网络环境不知道怎么回事，连接自己的香港服务器很慢，白天的时候还好，到了晚上就象蜗牛一样。套上了 Cloudflare 还是不行，套了 CDN 的网站加载起来莫名其妙的，点了链接一直没反应，加载的进度条也没有，然后突然就跳出来页面，很烦。于是就套个代理，想提高一下访问体验。。。
由于目前服务器使用的是 LNMP ，所以此次选择了 Nginx+Vmess+WS+TLS 的配置 ，当然，其他的像 Apache 和 Caddy 也都是可以的，架设步骤基本相似.废话不多说，我们开始：

<!--more-->

---

## 架设教程

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

##### 创建并编辑配置文件

###### 方法一

键入以下命令，编辑配置文件

```bash
vim /usr/local/etc/v2ray/config.json
```

###### 方法二

在本地新建名为 `config.json` 的文件，并进行编辑

###### 编辑配置文件

按照下面给出的标注位置修改即可，修改完成后删除标注

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
        "network": "ws",
        "wsSettings": {
          "path": "/path", // path 路径可自行设置
          "host": [
            "xx.com" // 要使用的域名
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

### 创建站点并配置 Nginx

#### 创建站点

键入以下命令，创建新站点

```bash
cd /root/oneinstack/
./vhost.sh
```

具体创建步骤请参考 [Oneinstack 开设站点](/p/oneinstack/#创建虚拟主机)

---

#### 配置 Nginx

键入以下命令，你将看到以你刚才所创建虚拟主机域名 .conf 命名的 Nginx 配置文件

```bash
cd /usr/local/nginx/conf/vhost/ && ls
```

然后我们键入下面的命令修改一下配置，文件名更换为你自己的域名 .conf

```bash
vim xx.com.conf
```

在最后一个 `}` 前面加入下面的内容，标注部分与 V2ray 配置文件保持一致

```nginx
location /path { # 与 V2Ray 配置中的 path 保持一致
      if ($http_upgrade != "websocket") {
          return 404;
      }
      proxy_redirect off;
      proxy_pass http://127.0.0.1:1234; # V2ray配置中WebSocket监听在环回地址的1234端口上
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
```
演示图如下：

{{< img src="/images/nginx-v2ray/1.png" caption="◎ Nginx 演示" position="center" width="700px" >}}

---

#### 扩展配置

##### Caddy2 配置

```caddyfile
xx.com {
    root * /var/www/html # 网站根目录
    @websockets {
		path /path # path路径
		header Connection *Upgrade*
		header Upgrade websocket
	  }
    reverse_proxy @websockets 127.0.0.1:1234 # 监听地址及端口
    tls xx@xx.com # 你的邮箱，用于签发证书
}
```

---

##### Apache 配置

若使用 Oneinstack 一键包，则和前文 Nginx 配置一样，加在最后 `</VirtualHost>` 前面即可

```apache
  <Location "/path/">
    ProxyPass ws://127.0.0.1:1234/yourair upgrade=WebSocket
    ProxyAddHeaders Off
    ProxyPreserveHost On
    RequestHeader append X-Forwarded-For %{REMOTE_ADDR}s
  </Location>
  ```

---

### 重启 Nginx

```bash
systemctl restart nginx.service
```

### 启动服务、添加开机自启

```bash
systemctl start v2ray.service
systemctl enable v2ray.service
```

至此，基于 Nginx 的 V2ray 服务便搭建完成。