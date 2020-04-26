---
title: Mac下使用Aria2
date: 2018-09-12 14:29:13
categories: Mac
tags:
    - Mac
---



# Mac下使用Aria2

## 参考教程

1. [Slark](https://slarker.me/mac-aria2/)
2. [张不二01](https://www.jianshu.com/p/77ca0f5b72cc)



## 安装教程

1. 如果没有安装homebrew，则使用下列命令安装

   ```shell
   # ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   
   ```

2. 使用 brew 安装 aria2

   ```shell
   # brew install aria2
   ```

3. 配置aria2    <!-- more -->

   * 创建`.aria2`文件夹，和`aria2.conf`配置文件

     ```shell
     # cd ~
     # mkdir .aria2
     # cd .aria2
     # touch aria2.conf
     # touch aria2.session
     ```
    ```
   
   * 修改配置文件
   
     ```shell
     # vim aria2.conf
    ```



     ```shell
     #用户名
     #rpc-user=user
     #密码
     #rpc-passwd=passwd
     #上面的认证方式不建议使用,建议使用下面的token方式
     #设置加密的密钥
     #rpc-secret=token
     #允许rpc
     enable-rpc=true
     #允许所有来源, web界面跨域权限需要
     rpc-allow-origin-all=true
     #允许外部访问，false的话只监听本地端口
     rpc-listen-all=true
     #RPC端口, 仅当默认端口被占用时修改
     #rpc-listen-port=6800
     # 最大同时下载数(任务数), 路由建议值: 3
     max-concurrent-downloads=5
     # 断点续传
     continue=true
     #同服务器连接数
     max-connection-per-server=5
     # 最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要
     min-split-size=10M
     # 单文件最大线程数, 路由建议值: 5
     split=10
     # 下载速度限制
     max-overall-download-limit=0
     # 单文件速度限制
     max-download-limit=0
     # 上传速度限制
     max-overall-upload-limit=0
     # 单文件速度限制
     max-upload-limit=0
     # 断开速度过慢的连接
     #lowest-speed-limit=0
     # 验证用，需要1.16.1之后的release版本
     #referer=*
     
     ## 文件相关
     
     # 文件保存路径, 默认为当前启动位置
     dir=/User/ding/Downloads/Aria2
     # 从会话文件中读取下载任务
     input-file=/Users/ding/Documents/Aria2/aria2.session
     # 在 Aria2 退出时保存 错误/未完成 的下载任务到会话文件
     save-session=/Users/ding/Documents/Aria2/aria2.session
     # 定时保存会话, 0 为退出时才保存, 需 1.16.1 以上版本, 默认:0
     save-session-interval=180
     
     #文件缓存, 使用内置的文件缓存
     #disk-cache=0
     # 另一种Linux文件缓存方式, 使用前确保您使用的内核支持此选项, 需要1.15及以上版本(?)
     #enable-mmap=true
     # 文件预分配, 能有效降低文件碎片, 提高磁盘性能. 缺点是预分配时间较长
     # 所需时间 none < falloc ? trunc << prealloc, falloc和trunc需要文件系统和内核支 
     file-allocation=prealloc
     
     ```



## 设置开机启动

1. 编写开机脚本

   ```shell
   # vim aria2.sh
   
   // 添加下列内容
   #!/bin/sh
   # Aria2 RPC #
   
   aria2c --conf-path="/Users/ding/.aria2/aria2.conf" -D 
    
   exit
   ```

2. 更改权限

   ```shell
   # chmod +x aria2.sh
   ```

3. 添加到开机自启

   `系统偏好设置` -> `用户与群组` -> `登陆项` 中把脚本添加进来并勾选隐藏



## Chrome浏览器的Aria2插件

* [Camtd - Aria2](https://chrome.google.com/webstore/detail/camtd-aria2-download-mana/lcfobgbcebdnnppciffalfndpdfeence?utm_source=InfinityNewtab)
* [BaiduExporter](https://github.com/acgotaku/BaiduExporter)
* [迅雷离线助手](https://chrome.google.com/webstore/detail/thunderlixianassistant/eehlmkfpnagoieibahhcghphdbjcdmen?hl=zh-CN)

