---
title: "一键 DD 自动重装脚本"
slug: "dd-sh"
date: 2022-12-01T10:53:36+08:00
lastmod: 2022-12-01T10:53:36+08:00
description: VPS 主机使用一键 DD 脚本自动重装系统优化版
enableTocContent: true
hidden: false
pinned: false
tags:
- vps
categories:
- 学习
image: /images/dd-sh/0.png
---

## 前言

VPS 主机一键自动重装系统脚本优化版，基于萌咖脚本优化、开发；DD 过程简单，傻瓜式安装。 脚本支持原版自定义密码安装，可选Linux和Windows系统，而且脚本支持Oracle甲骨文免费云主机使用支持uefi的镜像包安装，脚本已添加3个甲骨文专用选项。

一键DD脚本，支持性好，更智能更全面，支持国内外各种VPS重装，特别是对国内各种访问国外资源慢的VPS安装有奇效。

- <a href="https://github.com/fcurrk/reinstall" target="_blank">Github 项目地址</a>

## 安装过程

### 准备工作

为防止异常，先执行以下命令：

- Debian/Ubuntu:

```bash
apt update -y && apt dist-upgrade -y
```

- RedHat/CentOS:

```bash
yum makecache && yum update -y
```

安装重装系统的前提组件:

- Debian/Ubuntu:

```bash
apt-get install -y xz-utils openssl gawk file wget screen && screen -S os
```

- RedHat/CentOS:

```bash
yum install -y xz openssl gawk file glibc-common wget screen && screen -S os
```

---

### 一键 DD

执行命令：

```bash
wget --no-check-certificate -O NewReinstall.sh https://git.io/newbetags && chmod a+x NewReinstall.sh && bash NewReinstall.sh
```

如为CN主机(部分主机商已不能使用)，可能出现报错或不能下载脚本的问题，可执行以下命令开始安装：

```bash
wget --no-check-certificate -O NewReinstall.sh https://cdn.jsdelivr.net/gh/fcurrk/reinstall@master/NewReinstall.sh && chmod a+x NewReinstall.sh && bash NewReinstall.sh
```

运行后，会获取公网 IP、网关和子网掩码，确认一致后，按下 `Y` 回车下一步：
> 公网 IP 、网关及子网掩码可在 VPS 控制面板进行查看，若不一致，可输入 `N` 进行自定义

{{< img src="/images/dd-sh/1.png" caption="◎ 网络确认" position="center" >}}

然后进行系统选择，41合1的系统一键 DD 选择界面，输入99则使用自定义镜像：

{{< img src="/images/dd-sh/2.png" caption="◎ 系统选择" position="center" >}}

若输入 `9-16` 安装原版系统，可自定义密码，密码要求8-16位，以英文字母或数字开头，可以是大小写英文字母、数字及7个特殊字符.!$@#&%的任意组合，然后设置端口确认信息后，即可进行安装：

{{< img src="/images/dd-sh/3.png" caption="◎ 自定义设置" position="center" >}}

{{< notice warning "注意" >}}
谷歌云原版系统基础上DD会出现自动获取的子网掩码为255.255.255.255,如出现这种情况需要手工输入改正为正确的如255.255.255.0,否则会安装完成主机可能会离线。

阿里云因使用了特殊的驱动，DD安装Windows系统选择阿里云专用版。

Oracle Cloud（甲骨文云）可选择支持UEFI的镜像，注意基础系统最好选择Ubuntu，如原系统是CentOS可能无法成功，注意如是ARM机器注意选择同时支持ARM64和UEFI的镜像。
{{< /notice >}}

{{< expand "41合1系统密码" >}}
41合一系统密码：
1、CentOS 7.7 (已关闭防火墙及SELinux，默认密码Pwd@CentOS)
2、CentOS 7 (默认密码cxthhhhh.com)
3、CentOS 7 (支持ARM64、UEFI，默认密码cxthhhhh.com)
4、CentOS 8 (默认密码cxthhhhh.com)
5、Rocky 8 (默认密码cxthhhhh.com)
6、Rocky 8 (支持UEFI，默认密码cxthhhhh.com)
7、Rocky 8 (支持ARM64、UEFI，默认密码cxthhhhh.com)
8、CentOS 9 (默认密码cxthhhhh.com)
9、CentOS 6 (官方源原版，默认密码Minijer.com)
10、Debian 11 (官方源原版，默认密码Minijer.com)
11、Debian 10 (官方源原版，默认密码Minijer.com)
12、Debian 9 (官方源原版，默认密码Minijer.com)
13、Debian 8 (官方源原版，默认密码Minijer.com)
14、Ubuntu 20.04 (官方源原版，默认密码Minijer.com)
15、Ubuntu 18.04 (官方源原版，默认密码Minijer.com)
16、Ubuntu 16.04 (官方源原版，默认密码Minijer.com)
17、Windows Server 2022 (默认密码cxthhhhh.com)
18、Windows Server 2022 (支持UEFI，默认密码cxthhhhh.com)
19、Windows Server 2019 (默认密码cxthhhhh.com)
20、Windows Server 2016 (默认密码cxthhhhh.com)
21、Windows Server 2012 (默认密码cxthhhhh.com)
22、Windows Server 2008 (默认密码cxthhhhh.com)
23、Windows Server 2003 (默认密码cxthhhhh.com)
24、Windows 10 LTSC (默认密码Teddysun.com)
25、Windows 10 LTSC (支持UEFI，默认密码Teddysun.com)
26、Windows 7 x86 Lite (默认密码nat.ee)
27、Windows 7 x86 Lite (阿里云专用，默认密码nat.ee)
28、Windows 7 x64 Lite (默认密码nat.ee)
29、Windows 7 x64 Lite (支持UEFI，默认密码nat.ee)
30、Windows 10 LTSC Lite (默认密码nat.ee)
31、Windows 10 LTSC Lite (阿里云专用，默认密码nat.ee)
32、Windows 10 LTSC Lite (支持UEFI，默认密码nat.ee)
33、Windows Server 2003 Lite (C盘默认10G，默认密码WinSrv2003x86-Chinese)
34、Windows Server 2008 Lite (默认密码nat.ee)
35、Windows Server 2008 Lite (支持UEFI，默认密码nat.ee)
36、Windows Server 2012 Lite (默认密码nat.ee)
37、Windows Server 2012 Lite (支持UEFI，默认密码nat.ee)
38、Windows Server 2016 Lite (默认密码nat.ee)
39、Windows Server 2016 Lite (支持UEFI，默认密码nat.ee)
40、Windows Server 2022 Lite (默认密码nat.ee)
41、Windows Server 2022 Lite (支持UEFI，默认密码nat.ee)
99、自定义镜像
{{< /expand >}}

---

## 注意事项

系统名称后带Lite的均为精简版，没有的是完整版

[X64-Legacy-cxthhhhh]代表系统为AMD64位，支持传统BIOS启动，cxthhhhh定制的系统镜像

ARM64代表系统支持ARM64位

UEFI代表系统支持最新的UEFI启动，如甲骨文全部都是这种

aliyun代表阿里云专用系统镜像

cxthhhhh、teddysun、nat.ee均为三位制作系统镜像的大佬代称

系统密码会在选择相应序号后提示，请注意记录

系统安装过程会自动断开 SSH 连接，属于正常现象；等待安装完成后即可进行重连

## 参考资料

1. <a href="https://github.com/fcurrk/reinstall" target="_blank">一键 DD 脚本 Github 项目地址</a>

2. <a href="https://git.beta.gs/" target="_blank">一键 DD 脚本网址</a>