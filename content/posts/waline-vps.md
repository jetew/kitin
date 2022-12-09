---
title: "Waline 服务端独立部署方案"
slug: "waline-vps"
date: 2022-10-20T10:50:47+08:00
lastmod: 2022-10-23T10:50:47+08:00
description: 使用 Caddy+MariaDB/MySQL 在你的服务器上独立部署 Waline 服务端
hidden: false
comments: true
draft: false
toc: true
enableTocContent: true
weight: false
tags:
- hugo
- blog
- caddy
- mysql
- mariadb
- waline
categories:
- 学习
image: /images/waline-vps/0.png
---

{{< img src="/images/waline-vps/0.png" caption="◎ Waline" position="center" >}}

## 前言

之前讲了通过 Vercel 来进行 Waline 服务端的部署，这是最简单，也是完全免费的方法；但是对于拥有自己服务器的人来说，可能更想要将 Waline 直接部署到自己的服务器上；毕竟服务器摆在那，不用也是浪费，何不将 Waline 服务端也部署自己的服务器上以方便进行管理呢？其实 Waline 提供了很多的部署方案，可以看 <a href="https://waline.js.org/advanced/intro.html" target="_blank">这里</a> ；因为我一直使用的 Caddy 所以我就用 Caddy+MariaDB/MySQL 的方式来搭建，这里记录一下踩坑过程；当然你使用 Nginx/Apache 加上其他数据库的组合也是可以的。

<!--more-->

## 部署过程

### 准备工作

域名解析这块就不多讲了，提前绑定到你的服务器 IP ；服务器基本配置与优化可以参考 [这里](/p/setvps) ；至于 Web 服务这块，如果你使用 Nginx/Apache+MariaDB/MySQL 可以看 [这里](/p/oneinstack) 搭建好 Web 服务与数据库。

如果你想和我一样使用 Caddy Server 那么你可以参考 [这里](/p/caddy) 先搭建 Caddy Server ；然后来安装数据库，我这里以 MariaDB/MySQL 为例：

```bash
apt update
apt install mariadb-server -y
```

安装完 MariaDB 后，键入以下命令来进行初始化：

```bash
mysql_secure_installation
```

然后根据下面提示进行操作：

```bash
root@Jett:~# mysql_secure_installation

Enter current password for root (enter for none): # 输入数据库密码，由于并未设置，所以直接回车

Set root password? [Y/n] y # 是否设置数据库 root 密码
New password:
Re-enter new password: # 输入两次数据库 root 密码

Remove anonymous users? [Y/n] y # 是否移除匿名用户

Disallow root login remotely? [Y/n] y # 是否禁用 root 远程登陆

Remove test database and access to it? [Y/n] y # 是否移除测试数据库

Reload privilege tables now? [Y/n] y # 是否初始化数据库

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

如果你需要用软件进行远程管理数据库，那么就不要禁用远程登陆，并且需要通过防火墙来开放 `3306` 端口

---

### 安装 Nodejs

这里要注意，不要直接使用 `apt install` 进行安装，因为软件包版本比较低，与 **npm** 版本不匹配，从而导致后面报错。所以我们可以通过包管理器，或者二进制文件安装。

#### 包管理器安装

输入以下命令进行安装：

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - &&\
apt-get install -y nodejs
```

安装完成后可通过 `node-v` 和 `npm -v` 检验安装

卸载：

```bash
apt-get purge nodejs &&\
rm -r /etc/apt/sources.list.d/nodesource.list
```

#### 二进制文件安装

从 <a href="https://nodejs.org/" target="_blank">Nodejs官网</a> 下载二进制包，通过创建软链接至系统用户应用程序目录来使用。

打开 Nodejs 官网，点击 Downloads 然后右键复制 Linux Binaries (x64) 的下载链接：

{{< img src="/images/waline-vps/1.png" caption="◎ 二进制文件下载" position="center" width="700px" >}}

然后回到 SSH 软件，使用 `wget` 命令，下载到任意位置并进行解压然后删除压缩包；我这里以 **/usr/local/lib** 为例：

```bash
cd /usr/local/lib # 进入目录
mkdir nodejs && cd nodejs # 创建 nodejs 文件夹
wget https://nodejs.org/dist/v16.18.0/node-v16.18.0-linux-x64.tar.xz # 下载二进制包
tar -xvf node-v16.18.0-linux-x64.tar.xz # 解压
rm node-v16.18.0-linux-x64.tar.xz # 删除压缩包
cd node-v16.18.0-linux-x64 # 进入解压缩目录
mv * ../ # 将所有文件移动至上一目录
rm -r node-v16.18.0-linux-x64 # 删除解压缩目录
```

完成上述操作后，需要为 Nodejs 和 Npm 进行软链接到 **/usr/bin** 目录下

```bash
ln -s /usr/local/lib/nodejs/bin/node /usr/bin/node
ln -s /usr/local/lib/nodejs/bin/npm /usr/bin/npm
```

完成后可通过 `node -v` 和 `npm -v` 进行验证，返回版本号则表示成功。

---

### 安装 Waline 

按照官方给出的独立部署中直接运行的方案，进入你想要安装的位置，安装好模块后直接运行模块内的 vanilla.js 文件：

```bash
cd /usr/local/lib
npm install @waline/vercel
```

{{< notice warning "注意" >}}
国内服务器请先将 **npm** 设置为淘宝镜像！！！
{{< /notice >}}

#### 更换镜像

1. 临时更换

```bash
npm --registry https://registry.npm.taobao.org install @waline/vercel
```

2. 永久更换

```bash
npm config set registry https://registry.npm.taobao.org
```

3. 还原 npm 镜像

```bash
npm config set registry https://registry.npmjs.org/
```

> 2020-10-23 更新国内服务器设置淘宝镜像

下载完成后，可以试着运行一下：

```bash
node node_modules/@waline/vercel/vanilla.js
```

成功的话可以看到提示：

{{< img src="/images/waline-vps/2.png" caption="◎ 运行状态" position="center" width="700px" >}}

此时你可以通过 **IP:8360** 进行访问；不要着急，按捺下你的小激动，按下 <kbd><kbd>CTRL</kbd>+<kbd>C</kbd></kbd> 关闭，然后跟着我继续进行配置。

---

### 配置数据库

到官方文档的 <a href="https://waline.js.org/guide/server/databases.html" target="_blank">多数据库服务支持</a> 下载我们对应数据库的数据文件，我这里用的 MariaDB(MySQL) 所以下载 <a href="https://github.com/walinejs/waline/blob/main/assets/waline.sql" target="_blank">waline.sql</a> 然后进行导入数据：

> **注意**：将源代码直接复制然后自己进行编辑上传，不要直接使用 `wget` 进行下载

```bash
mysql -uroot -p # 登陆数据库
Enter password: # 输入密码
CREATE DATABASE waline; # 创建 waline 数据库
USE waline; # 指定数据库
source /path/waline.sql # 导入文件，/path改为你的路径
quit # 退出数据库
```

创建用户名，密码并赋予 waline 数据库的权限(可选)

```bash
mysql -uroot -p
Enter password:
CREATE USER 'waline'@'%' IDENTIFIED BY '密码';
GRANT ALL PRIVILEGES ON waline.* TO 'waline'@'%';
flush privileges;
quit
```

---

### 配置 systemd 服务

为了方便 Waline 的正常运行与管理，我使用的是 systemd 服务进行管理，创建文件并进行编辑(拿不准的可以在本地新建文件，编辑之后上传)：

```bash
cd /etc/systemd/system/
touch waline.service
vim waline.service
```

模板如下：

```service
[unit]
Description=Waline

[Service]
Type=simple
ExecStart=/usr/bin/node /usr/local/lib/node_modules/@waline/vercel/vanilla.js
Restart=always
User=root
Group=root
Environment=PATH=/usr/bin:/usr/local/bin
Environment=NODE_ENV=production
Environment=MYSQL_DB=数据库名
Environment=MYSQL_USER=用户名
Environment=MYSQL_PASSWORD=密码
WorkingDirectory=/usr/local/lib/node_modules/@waline/vercel

[Install]
WantedBy=multi-user.target
```

如果你之前按照我的步骤来安装 **Node** 和 **Npm** 还有 **Waline** 那么直接将脚本复制然后修改参数即可，如果你的位置和我不一样，那么就需要将 **ExecStart** 项中的路径换成你自己的。用户和组一般都是root，如果不确定可以使用命令查一下：

```bash
which node # node 安装位置
id -un # 查询用户
id -gn # 查询组
```

在 `[Service]` 项中，你可以通过 `Environment` 直接添加 Waline 插件的环境变量

保存/上传完毕后，让 systemd 重新载入单元文件，并启动 Waline ：

```bash
systemctl daemon-reload
systemctl start waline
```

此时再访问 http://IP:8360 ，你就能看到评论页面了。

一些常用的命令：

```bash
systemctl stop waline  # 停止服务
systemctl restart waline # 重启服务
systemctl status waline # 服务状态
systemctl enable waline # 添加开机自启动
```

---

### 反向代理设置

如果你不想通过 IP:8360 进行访问，那么就需要设置反向代理来通过域名进行访问了这里提供两个配置：

#### Caddy 

我使用的是 Caddy ， Caddy 反代需要修改 **/etc/caddy/Caddyfile** 文件：

```caddyfile
domain.com {
    root * /var/www/waline
    encode gzip
    reverse_proxy 127.0.0.1:8360
    file_server
    tls user@email.com
}
```

#### Nginx

Nginx 反向代理如下：

```nginx
location / {
    proxy_pass http://127.0.0.1:8360;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header REMOTE-HOST $remote_addr;
    add_header X-Cache $upstream_cache_status;
    # cache
    add_header Cache-Control no-cache;
    expires 12h;
  }
```

---

## 总结

以上就是我通过 Caddy+MariaDB 独立部署 Waline 的过程，希望能给大家带来帮助。后期如果需要设置邮件通知等功能可以在 waline.service 文件中的 **[Service]** 项下直接添加 Environment= 然后根据官方给出的环境变量进行设置即可。

若 Waline 后台发现有更新提示，可通过 `npm` 对 Waline 进行更新：

```bash
npm update @waline/vercel
```

### 参考资料

1. <a href="https://waline.js.org" target="_blank">Waline 官方文档</a>
   
2. <a href="https://github.com/nodesource/distributions/blob/master/README.md" target="_blank">NodeSource - Node.js 官方二进制发行版</a>
   
3. <a href="https://github.com/nodejs/help/wiki/Installation" target="_blank">通过二进制文件安装 Node.js</a>

4. <a href="https://www.ruanyifeng.com/blog/2016/03/node-systemd-tutorial.html" target="_blank">Node 应用的 Systemd 启动</a>