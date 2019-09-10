---
layout: inner
position: right
title: '小工具推荐之目录树结构生成/Tree'
date: 2019-09-01 10:15:00
categories: development
tags: Tools Tree
featured_image: '/img/posts/01_bloc-jams-angular-1130x864-2x.png'
project_link: ''
button_icon: 'github'
button_text: 'View'
lead_text: '在编写Markdown文档时，有时候需要结合项目的工程目录结构进行说明，特别是在培训的时候更加具体，更易被接受，这里推荐一款插件tree'
---

# 小工具推荐之目录树结构生成/Tree

## 前言:

在编写Markdown文档时，有时候需要结合项目的工程目录结构进行说明，特别是在培训的时候更加具体，更易被接受，这里推荐一款插件tree

安装

```bash
brew install tree
```

> 如果一直显示`Updating Hombrew... `，可以使用`control`+`c`停止更新，会直接执行需要下载的命令
>
> 或者使用`export HOMEBREW_NO_AUTO_UPDATE=true`不让其每次下载前检查自动更新

安装完成后使用`tree > tree.text`即可生成拥有当前目录结构的文本文件

![image-20190830183834428](/img/posts/mac/brew_tree.png)

可以把内容拷贝出来使用Markdown语法的代码块展示，效果如下，可自由添加注释说明 

```tex
.
├── bin
│   ├── cpdump
│   ├── dependencies-report
│   ├── ingest-convert.sh
│   ├── logstash
│   ├── logstash-plugin
│   ├── logstash-plugin.bat
│   ├── logstash.bat
│   ├── logstash.lib.sh
│   ├── ruby
│   ├── setup.bat
│   └── system-install
├── config
│   ├── jvm.options    
│   ├── log4j2.properties 
│   ├── logstash-mysql-es.conf 
│   ├── logstash.yml
│   └── startup.options
├── data
│   ├── dead_letter_queue
│   ├── queue
│   └── uuid
├── lib
│   └── ...
├── logs
│   └── ...
├── logstash-core
│   └── ...
├── mysql-conn
│   ├── mysql-connector-java-5.1.48.jar
│   └── sync.sql
├── tools
│   └── ...
└── vendor
    └── ...
    
```

> 如果想设置一些参数，比如像指定某些文件夹不显示，可以通过`tree -I XXX > tree.text`实现，
>
> 更多请通过`tree --help`查看帮助

```
usage: tree [-acdfghilnpqrstuvxACDFJQNSUX] [-H baseHREF] [-T title ]
	[-L level [-R]] [-P pattern] [-I pattern] [-o filename] [--version]
	[--help] [--inodes] [--device] [--noreport] [--nolinks] [--dirsfirst]
	[--charset charset] [--filelimit[=]#] [--si] [--timefmt[=]<f>]
	[--sort[=]<name>] [--matchdirs] [--ignore-case] [--fromfile] [--]
	[<directory list>]
  ------- Listing options -------
  -a            All files are listed.
  -d            List directories only.
  -l            Follow symbolic links like directories.
  -f            Print the full path prefix for each file.
  -x            Stay on current filesystem only.
  -L level      Descend only level directories deep.
  -R            Rerun tree when max dir level reached.
  -P pattern    List only those files that match the pattern given.
  -I pattern    Do not list files that match the given pattern.
  --ignore-case Ignore case when pattern matching.
  --matchdirs   Include directory names in -P pattern matching.
  --noreport    Turn off file/directory count at end of tree listing.
  --charset X   Use charset X for terminal/HTML and indentation line output.
  --filelimit # Do not descend dirs with more than # files in them.
  --timefmt <f> Print and format time according to the format <f>.
  -o filename   Output to file instead of stdout.
  ------- File options -------
  -q            Print non-printable characters as '?'.
  -N            Print non-printable characters as is.
  -Q            Quote filenames with double quotes.
  -p            Print the protections for each file.
  -u            Displays file owner or UID number.
  -g            Displays file group owner or GID number.
  -s            Print the size in bytes of each file.
  -h            Print the size in a more human readable way.
  --si          Like -h, but use in SI units (powers of 1000).
  -D            Print the date of last modification or (-c) status change.
  -F            Appends '/', '=', '*', '@', '|' or '>' as per ls -F.
  --inodes      Print inode number of each file.
  --device      Print device ID number to which each file belongs.
  ------- Sorting options -------
  -v            Sort files alphanumerically by version.
  -t            Sort files by last modification time.
  -c            Sort files by last status change time.
  -U            Leave files unsorted.
  -r            Reverse the order of the sort.
  --dirsfirst   List directories before files (-U disables).
  --sort X      Select sort: name,version,size,mtime,ctime.
  ------- Graphics options -------
  -i            Don't print indentation lines.
  -A            Print ANSI lines graphic indentation lines.
  -S            Print with CP437 (console) graphics indentation lines.
  -n            Turn colorization off always (-C overrides).
  -C            Turn colorization on always.
  ------- XML/HTML/JSON options -------
  -X            Prints out an XML representation of the tree.
  -J            Prints out an JSON representation of the tree.
  -H baseHREF   Prints out HTML format with baseHREF as top directory.
  -T string     Replace the default HTML title and H1 header with string.
  --nolinks     Turn off hyperlinks in HTML output.
  ------- Input options -------
  --fromfile    Reads paths from files (.=stdin)
  ------- Miscellaneous options -------
  --version     Print version and exit.
  --help        Print usage and this help message and exit.
  --            Options processing terminator.
```

