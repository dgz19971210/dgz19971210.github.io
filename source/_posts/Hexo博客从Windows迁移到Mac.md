---
title: Hexo博客从Windows迁移到Mac
date: 2018-08-17 16:45:38
categories: Hexo博客
tags:
    - Mac
---

# Hexo博客从Windows迁移到Mac
---
## 参考博客
[Gjincai](http://gjincai.github.io/2017/01/13/hexo-%E4%BB%8E-windows-%E8%BD%AC%E7%A7%BB%E8%87%B3-Mac/)

## 基本思路

1. 在Mac上安装好Hexo，初始化并生成博客目录
2. 生成新的SSH并添加到Github
3. 将旧博客覆盖新的博客<!-- more -->

## 具体实现

* 安装Node、git、和 Hexo(使用homebrew安装)

    1. 安装 Node：`brew install node`
    2. 安装 git：`brew install git`
    3. 安装 Hexo：`npm install hexo-cli -g`

* 生成 SSH key 并添加到 Github

    1. 检查是否存在SSH key：`cd ~/.ssh`  

    2. 生成 SSH key , " "中为GitHub绑定邮箱
        ```shell
            ssh-keygen -t rsa -C "xxxx@xxxx.com"
        ```

    3. 添加 SSH key 至 GitHub
       
        - GitHub 页面中：`Settings -> SSH and GPG keys`, 点击 `New SSH key`, 输入内容并保存。

        - 保存后悔向你的邮箱发送一个验证链接，点击验证即可。

        - 测试一下是否成功
            ```shell
                ssh git@github.com
            ```
* 部署博客，测试是否成功
  
    1. 把旧博客迁移到Mac中

        * 复制旧博客所有内容，覆盖到新博客文件夹

    2. 部署博客，测试是否成功

        ```shell
            hexo clean
            hexo d -g
        ```