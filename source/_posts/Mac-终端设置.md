---
title: Mac 终端设置
date: 2018-08-22 18:05:10
categories: Mac
tags:
    - Mac
    - 终端
---

# Mac终端设置

## 参考博客
[吾行者无疆](https://blog.csdn.net/u014057054/article/details/52124091)

## 教程
1. 复制一份vim配置模板到个人目录下
    ```shell
        cp /usr/share/vim/vimrc ~/.vimrc
    ```

2. 在配置文件中添加所需配置
    - 添加语法高亮和行号
        ```shell
            syntax on
            set nu!
        ```
    - 保存退出并验证

## .bash_profile常用配置
1. 设置别名

2. ```shell
   alias ll='ls -l'
   ```

3. 在标题栏上显示目录完整路径

   ```shell
   // 终端下输入以下两条命令
   defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES
   
   killall Finder
   ```


<!-- more -->