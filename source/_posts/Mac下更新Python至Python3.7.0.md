---
title: Mac下更新Python至Python3.7.0
date: 2018-09-05 01:50:39
categories: Mac
tags:
    - Mac
    - Python
---

# Mac下更新Python至Python3.7.0

前言：

        因Mac自带的Python版本为Python2.7.10，但Python2几乎要淘汰了，所以更新到Python3.7.0版本。



## 所需环境

1. 已安装Homebrew



## 更新步骤

<!-- more -->

1. 从Homebrew安装Python，默认安装最新版

   ```shell
   # brew install python
   ```



2. 设置别名

   ```shell
   打开 .bash_profile 文件
   # vim ~/.bash_profile
   
   添加别名，可用 which 查询python和python3的安装路径
   alias python2="/usr/bin/python"
   alias python3="/usr/local/bin/python3"
   alias python=python3
   
   ```



3. 使修改后的设置生效

   ```shell
   # source ~/.bash_profile
   ```

4. 测试Python的版本

   ```shell
   # pyhton -V       或    
   # python -version
   ```