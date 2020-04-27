---
title: VMWare 14 安装MacOS X 10.11.md
date: 2018-08-10 14:25:53
categories: Windows
tags:
    - Windows
    - Mac
    - 虚拟机
---
# VMWare 14 安装MacOS X 10.11
---

## 前言
因为需要Mac来调整一下plist文件。所以用虚拟机安装一下MacOS，本篇博客记录一下安装过程。

---
## 参考博客
[猫巷の博客](https://www.lovyou.top/post/51.html)

[moTzxx](https://blog.csdn.net/u011415782/article/details/78505422)

## 所需工具
1. 虚拟机（VMWare Workstations Pro 14）
2. Mac OS 镜像
3. Unlocker（虚拟机苹果破解补丁）
4. darwin.iso文件（需放到 VMWare14 的安装文件下）
5. OSX.vmx添加的配置：`smc.version = "0"` 

<!-- more -->

---

## 关键步骤
1. Win+R打开运行框，输入services.msc，然后关闭VM的所有服务
2. 解压Unlocker压缩包，找到win-install.cmd，然后右键以管理员身份运行
3. 普通虚拟机安装过程安装Mac
4. 安装遇到不可恢复错误则需修改客户机安装目录下的 OS X 10.11.VMX(客户机名.VMX) 文件，在 smc.present = "TRUE" 下添加下列行
    ```shell
        smc.version = "0"
    ```
5. 安装是提示磁盘空间不足，需要到 `实用工具->磁盘工具` 格式化一次安装盘
6. 安装VMWare Tools，若安装不上则需在虚拟机安装目录下添加 `darwin.iso` 文件，再将客户机的路径改到这个文件
7. 安装好VMWare Tools可以设置一个共享文件夹，硬件设置里右边的选项，然后设置共享文件夹