---
title: 搭建immich私有云相册
cover: date
banner:
  type: img
  bgurl: /img/common/{4B8E99AB-045D-9A54-B1D2-C51101220AA2}.jpg
  bannerText: 从安装服务器开始的搭建一个属于自己的相册备份方案
tags:
  - 笔记
categories: 笔记
date: 2024-03-13 22:50:54
---
> 本文记录了从安装ubuntu-server开始，到docker部署immich私有云相册的过程。包含了安装ubuntu-22.04.4-live-server，配置ubuntu局域网静态IP，安装docker，配置国内镜像，启动服务等一系列内容。
<!-- more -->

## 安装ubuntu-server

### 必做准备
1. 一个可以运行ubuntu的主机，可以是物理机，也可以是虚拟机，可以是docker容器，可以是云服务器，可以是任何可以运行ubuntu的设备。
2. 一个可以访问互联网的网络。
3. ubuntu-server的[官方镜像](https://cn.ubuntu.com/download/server/step1)，可以是ubuntu-22.04.4-live-server，也可以是ubuntu-22.04.4-server-amd64，具体看自己的需求。
4. 如果是物理机需要准备以下：
 - 一个空的U盘，存储最好不要小于4G。
 - [下载Rufus](https://rufus.ie/zh)，这是一款可以用于烧录镜像至U盘的工具。