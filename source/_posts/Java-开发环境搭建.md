---
title: Java 开发环境搭建
date: 2018-10-14 15:03:12
tags: 
    - Java
    - Mac
categories: Java
encrypt:
description:
---

{% cq %} 前言 {% endcq %}

{% note info %}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本篇博客介绍的是在 Mac 上搭建Java开发环境。其实无论是在什么操作系统下，搭建Java开发环境无非都是安装JDK，然后再修改环境变量
{% endnote %}

<!-- more -->

## 下载与安装 JDK

* 官网下载：[JDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)



## 配制环境变量

1. 在终端执行：`/usr/libexec/java_home -v`，查看JDK的安装位置

2. 编辑配置文件，写入 Java 环境变量的配置（zsh的可编辑`.zshrc`，bash可编辑`.bash_profile`）

   ```shell
   // JDK 1.8
   JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home
   JRE_HOME=$JAVA_HOME/jre
   PATH=$PATH:$JAVA_HOME/bin
   CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
   export JAVA_HOME
   export JRE_HOME
   export PATH
   export CLASSPATH
   ```

3. 终端输入：`source ~/.bash_profile` 应用变更

4. 测试 Java 环境是否搭建成功

   终端输入：`java -version`，查看所安装的JDK的版本



## 卸载 JDK

- 终端输入以下命令：

  ```shell
  sudo rm -rf /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
  
  sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane
  
  sudo rm -rf /Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk
  ```


## 参考文档

1. [lhajh](https://lhajh.github.io/mac/java/2017/11/05/mac-set-up-the-Java-development-environment.html)
2. [YouMeek](http://www.youmeek.com/mac-java/)

