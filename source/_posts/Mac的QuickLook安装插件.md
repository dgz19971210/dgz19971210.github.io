---
title: Mac的QuickLook安装插件
date: 2018-08-23 00:00:02
categories: Mac
tags:
    - Mac
---

# QuickLook安装插件
## 参考博客
[越前君](https://www.jianshu.com/p/7421f7a03e07)
[crazy一笑](https://www.jianshu.com/p/34d5e66dcf52)

## 安装Homebrew

- 终端输入：
    ```shell
    $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

    ```

- 若要卸载在终端把刚才的安装代码最后的install改为 `uninstall` 即可

<!-- more -->

## 安装QuickLook插件
1. 安装QLColorCode
    ```shell
        homebrew cask install qlcolorcode
    ```
    功能：预览代码语法高亮，（同时需要安装Highlight库，命令：`brew install highlight`）

2. 安装qlImageSize
    ```shell
        homebrew cask install qlimagesize
    ```
    功能：预览图片，同时显示图像大小和分辨率

3. 安装QLMarkdown
    ```shell
        homebrew cask install QLMarkdown
    ```
    功能：预览Markdown文件

4. 安装QuickLookJSON
    ```shell
        brew cask install quicklook-json
    ```
    功能：预览格式化的 JSON 文件

5. 安装QLStephen
    ```shell
        brew cask install qlstephen
    ```
    功能：预览无扩展名的纯文本文件

6. 安装BetterZipQL
    ```shell
        brew cask install betterzipql
    ```
    功能：预览Zip压缩文件的信息和目录

7. 安装QLVideo
    ```shell
        brew cask install qlvideo
    ```
    功能：预览 .mkv 等非原生支持的视频格式