---
layout: inner
position: right
title: 'Docker 实战（Mac）'
date: 2019-11-24 10:21:00
categories: development
tags: Docker
featured_image: '/img/posts/01_bloc-jams-angular-1130x864-2x.png'
project_link: ''
button_icon: 'github'
button_text: 'View'
lead_text: 'Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。'
---

[TOC]

# Docker 实战

## 一、简介

容器隔离包含8个方面

- PID命名空间 --进程标识符和能力
- UTS命名空间 --主机名和域名
- MNT命名空间 --文件系统访问和结构
- IPC命名空间  -- 通过共享内存的进程间通信
- NET命名空间 --网络访问和结构
- USR命名空间 --用户名和标识
- chroot（）--控制文件系统根目录的位置
- cgroups -- 资源保护

## 二、入门

### 1. run hello_world （--detach or -d 以守护方式运行，后台启动）

```shell
 ~ $: docker run dockerinaction/hello_world
Unable to find image 'dockerinaction/hello_world:latest' locally
latest: Pulling from dockerinaction/hello_world
a3ed95caeb02: Pulling fs layer
1db09adb5ddd: Downloading
latest: Pulling from dockerinaction/hello_world
a3ed95caeb02: Pull complete
1db09adb5ddd: Pull complete
Digest: sha256:cfebf86139a3b21797765a3960e13dee000bcf332be0be529858fca840c00d7f
Status: Downloaded newer image for dockerinaction/hello_world:latest
hello world  //启动成功，输出
```

2. ### docker images (查看本地镜像)

   ```shell
    ~ $: docker images
   REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
   hello-world                  latest              fce289e99eb9        6 months ago        1.84kB
   dockerinaction/hello_world   latest              a1a9a5ed65e9        3 years ago         2.43MB
   ```

   