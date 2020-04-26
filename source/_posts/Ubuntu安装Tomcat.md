---
title: Ubuntu安装Tomcat
date: 2019-02-28 19:54:43
tags: 
    - Ubuntu
    - Linux
    - Java
categories: Linux
encrypt:
description:
---

{% cq %} 前言 {% endcq %}

{% note info %}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ubuntu 安装 Tomcat
{% endnote %}

<!-- more -->

# Ubuntu 安装 Tomct

1. 官网下载 Linux 对应的安装包

   - [官网](http://tomcat.apache.org/download-80.cgi)

2. 上传并解压缩，移动到 `/usr/local/tomcat8`，设置所有者

   ```shell
   # 解压
   tar -zxvf apache-tomcat-8.5.23.tar.gz
   # 重命名
   mv apache-tomcat-8.5.23 tomcat
   # 移动目录
   mv tomcat /usr/local/tomcat8/
   ```

3. Tomcat常用命令

   ```shell
   # 启动
   /usr/local/tomcat/bin/startup.sh
   
   # 停止
   /usr/local/tomcat/bin/shutdown.sh
   ```

   

