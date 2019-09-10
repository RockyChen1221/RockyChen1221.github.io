---
layout: inner
position: right
title: 'Logstash同步MySQL数据到ElasticSearch'
date: 2019-09-09 19:15:00
categories: development
tags: Logstash MySQL ElasticSearch
featured_image: '/img/posts/01_bloc-jams-angular-1130x864-2x.png'
project_link: ''
button_icon: 'github'
button_text: 'View'
lead_text: '使用Logstash同步MySQL数据到ElasticSearch'
---
## 使用Logstash同步MySQL数据到ElasticSearch

> 这里选择用logstash同步

安装 jdbc 和 elasticsearch 插件，在解压后的logstash目录中执行

```shell
bin/logstash-plugin install logstash-input-jdbc //在高版本自带有input-jdbc插件
bin/logstash-plugin install logstash-output-elasticsearch
```

> 此处需要关闭VPN等服务，否则报I/O exception (java.net.SocketException) 443：Operation timed out (Read failed)

如图

![image](/img/posts/logstash/logstash-plugins.png)


进入logstash/config目录，新增一个文件，内容如下

```shell
input {
  jdbc {
    type => "demo"  #△
    # mysql相关jdbc配置
    jdbc_connection_string => "jdbc:mysql://192.168.1.187:3306/demo?useUnicode=true&characterEncoding=utf-8&useSSL=false"
    jdbc_user => "demo"
    jdbc_password => "demo"

    # jdbc连接mysql驱动的文件目录，可去官网下载:https://dev.mysql.com/downloads/connector/j/
    jdbc_driver_library => "mysql-conn/mysql-connector-java-5.1.48.jar"
    # the name of the driver class for mysql
    jdbc_driver_class => "com.mysql.jdbc.Driver"
    jdbc_paging_enabled => true
    jdbc_page_size => "50000"

    jdbc_default_timezone =>"Asia/Shanghai"

    # mysql文件, 也可以直接写SQL语句在此处，如下：
    # statement => "select * from ei_invest_base_info where d_update_time > :sql_last_value"
    statement_filepath => "mysql-conn/sync.sql"

    # 这里类似crontab,可以定制定时操作，比如每分钟执行一次同步(分 时 天 月 年)
    schedule => "* * * * *"
    #type => "jdbc"

    # 是否记录上次执行结果, 如果为真,将会把上次执行到的 tracking_column 字段的值记录下来,保存到 last_run_metadata_path 指定的文件中
    #record_last_run => true

    # 是否需要记录某个column 的值,如果record_last_run为真,可以自定义我们需要 track 的 column 名称，此时该参数就要为 true. 否则默认 track 的是 timestamp 的值.
    use_column_value => true

    # 如果 use_column_value 为真,需配置此参数. track 的数据库 column 名,该 column 必须是递增的. 一般是mysql主键
    tracking_column => "d_update_time"

    tracking_column_type => "timestamp"

    last_run_metadata_path => "./mysql-conn/demo_sync_last_id"

    # 是否清除 last_run_metadata_path 的记录,如果为真那么每次都相当于从头开始查询所有的数据库记录
    clean_run => false

    #是否将 字段(column) 名称转小写
    lowercase_column_names => false
  }
}

output {
 if [type]=="demo" {
  elasticsearch {
    hosts => "127.0.0.1:9200"
    index => "demo"          # index
    document_type => "info"  # 分类
    document_id => "%{id}"   # 此处的ID为SQL中需要用作标识的列
    template_overwrite => true
  }

  # 这里输出调试，正式运行时可以注释掉
  stdout {
      codec => json_lines
  }
 }
}
```

> 注意：需要修改的地方为

回到Logstash根目录，执行`bin/logstash -f config/新增文件名`