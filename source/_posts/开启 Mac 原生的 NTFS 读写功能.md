---
layout: 开启
title: 开启 Mac 原生的 NTFS 读写功能
date: 2018-12-01 22:39:13
categories: Mac
tags:
    - Mac
---

## {% cq %}前言{% endcq %}

<p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用苹果系统，在读取 NTFS 格式的U盘和硬盘的时候，总是各种不方便，要么就是一些付费的软件, 要么就是各种的时灵时不灵，一大堆烦恼。我这里记录以下 Mac 自带的 NTFS 读取和写入功能。

</p>

<!-- more -->

## 查看盘符

* 查看需要读取和写入的 NTFS 格式的磁盘的盘符。

  ```shell
  diskutil list
  ```

  注：在终端下输入此命令，使用磁盘工具列出磁盘列表，查看盘符名

## 修改/etc/fstab

* 终端下输入

  ```shell
  sudo nano /etc/fstab
  ```

* 添加一行

  ```
  LABEL=盘符 none ntfs rw,auto,nobrowse
  ```

  注：吐过盘符名中间有空格，用`\040`代替

* 按 Ctrl + X 保存
* 按 Enter 回到终端开始界面

## 重启

