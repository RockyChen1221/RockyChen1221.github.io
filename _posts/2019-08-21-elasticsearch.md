---
layout: inner
position: right
title: '初识ElasticSearch（Mac）'
date: 2019-08-21 14:15:00
categories: development
tags: ElasticSearch
featured_image: ''
project_link: ''
button_icon: 'github'
button_text: 'View'
lead_text: 'ElasticSearch是一个基于[Lucene](https://baike.baidu.com/item/Lucene/6753302)的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java语言开发的，并作为Apache许可条款下的开放源码发布，是一种流行的企业级搜索引擎。'
---


# 初识ElasticSearch（Mac）

## 1. 简介

ElasticSearch是一个基于[Lucene](https://baike.baidu.com/item/Lucene/6753302)的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java语言开发的，并作为Apache许可条款下的开放源码发布，是一种流行的企业级搜索引擎。ElasticSearch用于[云计算](https://baike.baidu.com/item/云计算/9969353)中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。官方客户端在Java、.NET（C#）、PHP、Python、Apache Groovy、Ruby和许多其他语言中都是可用的。根据DB-Engines的排名显示，Elasticsearch是最受欢迎的企业搜索引擎，其次是Apache Solr，也是基于Lucene。

### 1.1 与MySQL的区别

| ElasticSearch    | Mysql              |
| ---------------- | ------------------ |
| 索引（index）    | 数据库（database） |
| 类型（type）     | 表（table）        |
| 列（field）      | 属性/列（column）  |
| 文档（document） | 数据（data）       |

> 注意事项： 不建议使用type 因为按照elastic search的计划后面的版本7.0以后会移除type

### 1.2 ELK是什么

ELK=elasticsearch+Logstash+kibana 
elasticsearch：后台分布式存储以及全文检索 
logstash: 日志加工、“搬运工” 
kibana：数据可视化展示。 
ELK架构为数据分布式存储、可视化查询和日志解析创建了一个功能强大的管理链。 三者相互配合，取长补短，共同完成分布式大数据处理工作。

### 1.3 前置条件

- ES 5，安装需要 JDK 8 以上
- ES 6.5，安装需要 JDK 11 以上
- ES 7.2.1，内置了 JDK 12

## 2. Brew安装

```bash
brew install elasticsearch
```

安装成功后会出现如下目录结构

```bash
==> /usr/local/Cellar/elasticsearch/6.8.1/bin/elasticsearch-keystore create
==> Caveats
Data:    /usr/local/var/lib/elasticsearch/
Logs:    /usr/local/var/log/elasticsearch/elasticsearch_datadriver.log
Plugins: /usr/local/var/elasticsearch/plugins/
Config:  /usr/local/etc/elasticsearch/

To have launchd start elasticsearch now and restart at login:
  brew services start elasticsearch
Or, if you don't want/need a background service you can just run:
  elasticsearch
==> Summary
```

使用`elasticsearch --version`查看当前版本

```shell
Java HotSpot(TM) 64-Bit Server VM warning: Cannot open file logs/gc.log due to No such file or directory

Version: 6.8.1, Build: oss/tar/1fad4e1/2019-06-18T13:16:52.517138Z, JVM: 1.8.0_211
```

启动 `elasticsearch`，默认端口是9200，http://127.0.0.1:9200/ 看到ES的基本信息

```json
{
    "name": "YtPiknS",
    "cluster_name": "elasticsearch_datadriver",
    "cluster_uuid": "eeXuM5z_T7KNk_QZgatJNQ",
    "version": {
        "number": "6.8.1",
        "build_flavor": "oss",
        "build_type": "tar",
        "build_hash": "1fad4e1",
        "build_date": "2019-06-18T13:16:52.517138Z",
        "build_snapshot": false,
        "lucene_version": "7.7.0",
        "minimum_wire_compatibility_version": "5.6.0",
        "minimum_index_compatibility_version": "5.0.0"
    },
    "tagline": "You Know, for Search"
}
```

## 3. 安装Kibana

Kibana是ES的一个配套工具，让用户在网页中可以直接与ES进行交互。 

```shell
brew install kibana //安装
kibana //启动 默认端口5601
```

启动成功后 http://localhost:5601 

## 4. 安装Head

4.1.  下载[github下载地址](https://github.com/mobz/elasticsearch-head#running-with-built-in-server)  注：需要node环境

4.2.  `npm run start `启动 ，默认地址为`locahost:9100`

4.3.  建立和ES的关联，修改`config/elasticsearch.yml`，添加

```yaml
# head plugins
http.cors.enabled: true
http.cors.allow-origin: "*"
```

4.4. 建立集群，编辑`config/elasticsearch.yml`，添加

```shell
cluster.name: XXX  //集群名称
node.name: master  //节点名称
node.master: true  //是否master节点

network.host: 127.0.0.1 //绑定的IP地址，端口默认是9200
```

Window+Mac 集群配置

4.4.1. Windows (IP: 192.168.0.105) (作为集群master)

![image-total](/img/posts/es/windows_yml.jpg){:height="auto" width="780"}

![image-total](/img/posts/es/windows_logs.png){:height="auto" width="780"}

![image-total](/img/posts/es/windows.png){:height="auto" width="780"}

![image-total](/img/posts/es/windows_nodes.png){:height="auto" width="780"}

4.4.2. Mac (IP: 192.168.0.101) 

![image-total](/img/posts/es/mac_yml.png){:height="auto" width="780"}

![image-total](/img/posts/es/mac_logs.png){:height="auto" width="780"}

![image-total](/img/posts/es/mac.png){:height="auto" width="780"}

![image-total](/img/posts/es/mac_nodes.png){:height="400" width="780"}

## 5 .踩坑

1. Error: Caused by: java.lang.IllegalStateException: failed to obtain node locks
	绑定节点失败，原因由ES未正常关闭引起所致，通过`ps aux | grep 'elastic' ` 查找对应进程，通过`kill -9 进程号` 结束进程，或者使用`jps` 查找，最后重新启动

2. max virtual memory areas vm.max*map*count [65530] is too low

   ```shell
   sudo sysctl -w vm.max_map_count=262144
   ```

3. max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]

   ```shell
   ulimit -Hn  //查看硬限制大小
   su root     //切换root编辑
   vi /etc/security/limits.conf 
   //添加以下内容，其中*可设置为指定用户，ES从5开始不支持root用户启动，保存退出后重新 ulimit -Hn
   * soft nofile 65536
   * hard nofile 65536
   ```
4. 集群过程中机器需要双向互通

## 6.  参阅

[阮一峰的网络日志](http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html)

[Elasticsearch学习](https://blog.csdn.net/makang110/article/details/80596017)

注：本文使用版本为5.6.16，上面安装使用的是6.8.1，后面实际使用的是5.6.16，考虑到版本问题以及jdk,所以和公司保持一致





