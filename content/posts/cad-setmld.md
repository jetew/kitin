---
title: "自定义 CAD 引线标注样式"
slug: cad-setmld
date: 2022-10-27T14:46:27+08:00
lastmod: 2022-10-27T14:46:27+08:00
description: 自定义 CAD 箭头、索引符，并设置引线标注样式
hidden: false
comments: true
draft: false
toc: true
enableTocContent: true
weight: false
tags:
- cad
categories:
- 笔记
image: /images/cad-setmld/0.png
---

{{< img src="/images/cad-setmld/0.png" caption="◎ 引线标注" position="center" >}}

## 前言

近期由于工作原因，更换了统一制图模板，但是有一些东西不是很满意。比如箭头和引线标注。。。于是决定自己定义下，遂在此进行简单的记录。

<!--more-->

## 设置步骤

### 箭头设置

`CTRL+N` 新建文件，选择 **"公制 (M)"** 模板文件：

{{< img src="/images/cad-setmld/1.png" caption="◎ 新建模板" position="center" width="700px" >}}

绘制引线所用的自定义箭头，绘制完成后进行填充：

{{< img src="/images/cad-setmld/2.png" caption="◎ 绘制箭头" position="center" width="700px" >}}

使用 `B` 定义块命令，将箭头制作成块，**基点** 选择箭头右边的角：

{{< img src="/images/cad-setmld/3.png" caption="◎ 定义箭头块" position="center" width="700px" >}}

自定义箭头设置完成~~

### 索引符设置

绘制引线标注所用的索引符号：

{{< img src="/images/cad-setmld/4.png" caption="◎ 绘制索引符" position="center" width="700px" >}}

使用`ST` 文字样式设置，创建新的字体样式，字体选择 **CityBluepeint** ，名称随意；并置为当前：

{{< img src="/images/cad-setmld/5.png" caption="◎ 文字样式" position="center" width="700px" >}}

使用`ATT` 属性定义，创建属性标记为 **标高** ，文字对正为 **正中 (注意图中参数设置)**：

{{< img src="/images/cad-setmld/6.png" caption="◎ 属性定义" position="center" width="700px" >}}

然后将其放在右上角矩形正中位置：

{{< img src="/images/cad-setmld/7.png" caption="◎ 放置位置" position="center" width="700px" >}}

使用`ATT` 属性定义，创建属性标记为 **材料缩写** ，文字对正为 **正中 (注意图中参数设置)**：

{{< img src="/images/cad-setmld/8.png" caption="◎ 属性定义" position="center" width="700px" >}}

然后将其放在左下角矩形正中位置：

{{< img src="/images/cad-setmld/9.png" caption="◎ 放置位置" position="center" width="700px" >}}

使用`ATT` 属性定义，创建属性标记为 **材料编号** ，文字对正为 **正中 (注意图中参数设置)**：

{{< img src="/images/cad-setmld/10.png" caption="◎ 属性定义" position="center" width="700px" >}}

然后将其放在右下角矩形正中位置：

{{< img src="/images/cad-setmld/11.png" caption="◎ 放置位置" position="center" width="700px" >}}

使用 `POL` 在左上角矩形中心绘制一个正三角形，并进行填充：

{{< img src="/images/cad-setmld/12.png" caption="◎ 绘制正三角" position="center" width="700px" >}}

使用 `ATT` 属性定义，创建属性标记为 **材料名称** ，文字对正为 **正中 (注意图中参数设置)**：

{{< img src="/images/cad-setmld/13.png" caption="◎ 属性定义" position="center" width="700px" >}}

将其放置于大矩形下面那条边的中心位置：

{{< img src="/images/cad-setmld/14.png" caption="◎ 放置位置" position="center" width="700px" >}}

然后使用 `M` 将其下移 4.5 ：

{{< img src="/images/cad-setmld/15.png" caption="◎ 移动位置" position="center" width="700px" >}}

使用 `B` 定义块命令，将其制作成块，注意 **基点** 位置：

{{< img src="/images/cad-setmld/16.png" caption="◎ 定义块" position="center" width="700px" >}}

完成后点击确定，就可以看到索引符已经创建完：

{{< img src="/images/cad-setmld/17.png" caption="◎ 索引符" position="center" width="700px" >}}

### 设置引线样式

使用 `MLS` 命令，打开多重引线样式设置，并进行新建：

{{< img src="/images/cad-setmld/18.png" caption="◎ 新建样式" position="center" width="700px" >}}

找到 **箭头 - 符号** 选择用户箭头：

{{< img src="/images/cad-setmld/19.png" caption="◎ 设置箭头" position="center" width="700px" >}}

选择刚才制作的自定义箭头，并将箭头大小设置为 **1**：

{{< img src="/images/cad-setmld/20.png" caption="◎ 箭头大小" position="center" width="700px" >}}

然后点击 **内容 - 多重引线类型** 将其设置为 **块** ；然后块源选择 **用户块**，并设置为刚才制作的 **索引符**：

{{< img src="/images/cad-setmld/21.png" caption="◎ 设置内容" position="center" width="700px" >}}

接着点击 **引线结构** 将基线距离设置为 **3** ：

{{< img src="/images/cad-setmld/22.png" caption="◎ 基线距离" position="center" width="700px" >}}

完事之后确定，并且置为当前，引线样式设置完毕~~

## 效果展示

使用 `MLD` 来标记一个引线试试看吧：

{{< img src="/images/cad-setmld/23.png" caption="◎ 编辑属性" position="center" width="700px" >}}

{{< img src="/images/cad-setmld/24.png" caption="◎ 最终效果" position="center" width="700px" >}}