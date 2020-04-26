---
title: Ubuntu安装JDK
date: 2019-02-28 19:54:34
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
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ubuntu 安装 JDK
{% endnote %}

<!-- more -->

# Ubuntu 安装 JDK

1. 官网下载 Linux 对应的安装包

   * [官网](<http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>)

2. 上传并解压缩，移动到 `/usr/local/java`，设置所有者

   ```shell
   # 解压
   tar -zxvf jdk-8u152-linux-x64.tar.gz 
   # 创建目录
   mkdir -p /usr/local/java
   # 移动到java目录
   mv jdk1.8.0_152/ /usr/local/java/
   # 设置所有者
   chown -R root:root /usr/local/java
   ```

3. 配置环境变量

   ```shell
   vim /etc/enviroment
   ```

   添加如下配置

   ```shell
   PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
   export JAVA_HOME=/usr/local/java/jdk1.8.0_152
   export JRE_HOME=/usr/local/java/jdk1.8.0_152/jre
   export CLASSPATH=$CLASSPATH:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
   ```

   配置用户环境变量

   ```shell
   vim /etc/profile
   ```

   添加如下配置

   ```shell
   if [ "$PS1" ]; then
     if [ "$BASH" ] && [ "$BASH" != "/bin/sh" ]; then
       # The file bash.bashrc already sets the default PS1.
       # PS1='\h:\w\$ '
       if [ -f /etc/bash.bashrc ]; then
         . /etc/bash.bashrc
       fi
     else
       if [ "`id -u`" -eq 0 ]; then
         PS1='# '
       else
         PS1='$ '
       fi
     fi
   fi
   
   export JAVA_HOME=/usr/local/java/jdk1.8.0_152
   export JRE_HOME=/usr/local/java/jdk1.8.0_152/jre
   export CLASSPATH=$CLASSPATH:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
   export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH:$HOME/bin
   
   if [ -d /etc/profile.d ]; then
     for i in /etc/profile.d/*.sh; do
       if [ -r $i ]; then
         . $i
       fi
     done
     unset i
   fi
   ```

   使配置生效

   ```shell
   source /etc/profile
   ```

   测试是否安装成功

   ```
   root@UbuntuBase:/usr/local/java# java -version
   java version "1.8.0_152"
   Java(TM) SE Runtime Environment (build 1.8.0_152-b16)
   Java HotSpot(TM) 64-Bit Server VM (build 25.152-b16, mixed mode)
   ```

   为其他用户更新环境变量

   ```shell
   # 切换用户
   su dylan
   # 更新环境变量
   source /etc/profile
   ```

   