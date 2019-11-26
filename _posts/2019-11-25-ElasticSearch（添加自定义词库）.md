---
layout: inner
position: right
title: 'ElasticSearch（添加自定义词库）'
date: 2019-11-25 11:21:00
categories: development
tags: ElasticSearch IK分词器
featured_image: '/img/posts/01_bloc-jams-angular-1130x864-2x.png'
project_link: ''
button_icon: 'github'
button_text: 'View'
lead_text: '在进行中文搜索的时候，一般常用的使用`IK分词器`，但对于一些场景下的分词搜索结果并不太满意，原因是词库的数据不完整，所以需要添加自定义词库'
---

[TOC]

# ElasticSearch（添加自定义词库）

前言:

在进行中文搜索的时候，一般常用的使用`IK分词器`，但对于一些场景下的分词搜索结果并不太满意，原因是词库的数据不完整，所以需要添加自定义词库

## 1. 安装IK分词器

 下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases ，下载对应版本的IK解压至ES目录下的`plugins`即可

## 2. 编写自定义词库文件my.dic

进入analysis-ik目录下的config，新建一个文件my.dic，文件内容格式：一行一个词即可

## 3 . 修改IKAnalyzer.cfg.xml，然后重启

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">;
<properties>
        <comment>IK Analyzer 扩展配置</comment>
        <!--用户可以在这里配置自己的扩展字典 -->
        <entry key="ext_dict">my.dic</entry>
        <!--用户可以在这里配置自己的扩展停止词字典-->
        <entry key="ext_stopwords"></entry>
        <!--用户可以在这里配置远程扩展字典 -->
        <!-- <entry key="remote_ext_dict">words_location</entry> -->
        <!--用户可以在这里配置远程扩展停止词字典-->
        <!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>
```

## 4. 参考

[热更新词库](https://www.cnblogs.com/zlslch/p/6441315.html?utm_source=itdadao&utm_medium=referral)