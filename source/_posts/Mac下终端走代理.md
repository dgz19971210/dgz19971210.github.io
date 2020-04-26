---
title: Mac下终端走代理
date: 2018-08-23 00:46:00
categories: Mac
tags:
    - Mac
---

# Mac下终端走代理
---

## 参考教程
[fazero](https://blog.fazero.me/2015/09/15/%E8%AE%A9%E7%BB%88%E7%AB%AF%E8%B5%B0%E4%BB%A3%E7%90%86%E7%9A%84%E5%87%A0%E7%A7%8D%E6%96%B9%E6%B3%95/)

## 当前终端走代理
- 在终端输入下列内容：
    ```shell
        export http_proxy=http://127.0.0.1:1080
    ```
- address和port替换为代理程序相应的地址好端口

## 在shell配置文件 .bashrc 或者 .zshrc 中设置
- 直接在 `.bashrc` 或者 `.zshrc` 添加下面内容，地址与端口根据自己的代理程序修改
    ```shell
        export http_proxy="http://127.0.0.1:1080"
        export https_proxy="http://127.0.0.1:1080"
    ```
- 保存并在终端执行：`source ~/.bashrc` ，此方法永久有效

<!-- more -->