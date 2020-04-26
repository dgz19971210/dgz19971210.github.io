---
title: VPS 搭建 SSR 和启用 BBR plus
date: 2019-03-31 11:55:47
tags:
    - vps
    - vpn
categories: vpn
encrypt:
description:
---

{% cq %} 前言 {% endcq %}

{% note info %}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;谷歌云 VPS 搭建 SSR 和启用 BBR plus
{% endnote %}

<!-- more -->

## 修改登录密码

1. 创建实例后，在控制台连接实例

2. 输入命令，进入 root 用户

   ```shell
   sudo -i
   ```

3. 更改 root 用户密码

   ```shell
   passwd
   ```

   

## 开启远程登录权限

使用其中一种即可

### 方法一

1. 进入ssh 配置文件

   ```shell
   vim /etc/ssh/sshd_config
   ```

2. 修改文件内容如下

   开启 ROOT 用户登录

   ```shell
   PermitRootLogin yes
   ```

   开启密码验证

   ```shell
   PasswordAuthentication yes
   ```

3. 重启 ssh 服务

   ```shell
   /etc/init.d/ssh restart
   ```

### 方法二

执行一键命令

```shell
sed -i 's/^.*PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config &&\
sed -i 's/^.*PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config &&\ 
/etc/ssh/sshd_config
```





## 搭建 SSR

### flyzy 一键脚本

参考：[GitHub](https://github.com/flyzy2005/ss-fly)

1. 下载一键搭建ssr脚本

   ```shell
   git clone https://github.com/flyzy2005/ss-fly
   ```

2. 运行一键脚本

   ```shell
   ss-fly/ss-fly.sh -ssr
   ```

3. 输入对应的参数

   可输入相应序号，也可以直接回车选择默认

4. 相关操作ssr命令

   ```shell
   启动：/etc/init.d/shadowsocks start
   停止：/etc/init.d/shadowsocks stop
   重启：/etc/init.d/shadowsocks restart
   状态：/etc/init.d/shadowsocks status
    
   配置文件路径：/etc/shadowsocks.json
   日志文件路径：/var/log/shadowsocks.log
   代码安装目录：/usr/local/shadowsocks
   ```

   

5. 卸载ssr服务

   ```shell
   ./shadowsocksR.sh uninstall
   ```



### 秋水逸冰一键脚本

参考： [秋水逸冰](https://teddysun.com/486.html)

1. 使用一键脚本安装 SSR

   ```shell
   wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
   chmod +x shadowsocks-all.sh
   ./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
   ```

   

2. 卸载方法

   若安装多种需要运行多次

   ```shell
   ./shadowsocks-all.sh uninstall
   ```

   

3. 相关操作命令

   ```shell
   Shadowsocks-Python 版：
   /etc/init.d/shadowsocks-python start | stop | restart | status
   
   ShadowsocksR 版：
   /etc/init.d/shadowsocks-r start | stop | restart | status
   
   Shadowsocks-Go 版：
   /etc/init.d/shadowsocks-go start | stop | restart | status
   
   Shadowsocks-libev 版：
   /etc/init.d/shadowsocks-libev start | stop | restart | status
   ```



4. 配置文件所在地

   ```shell
   Shadowsocks-Python 版：
   /etc/shadowsocks-python/config.json
   
   ShadowsocksR 版：
   /etc/shadowsocks-r/config.json
   
   Shadowsocks-Go 版：
   /etc/shadowsocks-go/config.json
   
   Shadowsocks-libev 版：
   /etc/shadowsocks-libev/config.json
   ```

   



## 开启 BBR plus

> 使用其中一种

### 四合一加速脚本

![](https://s1.ax1x.com/2018/12/24/F6XveP.png)

没有 wget 的需要用软件包管理工具安装，命令：`yum install -y wget`



```shell
wget "https://github.com/chiakge/Linux-NetSpeed/raw/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```



项目地址：[GitHub](https://github.com/cx9208/Linux-NetSpeed)



### 一键 CentOS 脚本

**项目地址**：[GitHub](https://github.com/cx9208/bbrplus)



**一键脚本**：

```shell
wget "https://github.com/cx9208/bbrplus/raw/master/ok_bbrplus_centos.sh" && chmod +x ok_bbrplus_centos.sh && ./ok_bbrplus_centos.sh
```