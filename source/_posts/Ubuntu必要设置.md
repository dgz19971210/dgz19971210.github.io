---
title: Ubuntu必要设置
date: 2019-02-28 16:54:47
tags: 
    - Ubuntu
    - Linux
categories: Linux
encrypt:
description:
---

{% cq %} 前言 {% endcq %}

{% note info %}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ubuntu系统对网卡设置静态地址和设置时区
{% endnote %}

<!-- more -->

## Ubuntu设置网卡静态地址

1. 进入网卡配置文件
    ```shell
    vim /etc/netplan/50-cloud-init.yaml
    ```

2. 修改配置如下
    ```shell
    network:
        ethernets:
            enp0s3:
                addresses: [10.3.133.33/24]
                gateway4: 10.3.133.1
                dhcp4: no
                dhcp6: no
                nameservers:
                    addresses: [10.3.133.1,114.114.114.114]
        version: 2         
    ```

3. 使配置生效
    ```shell
    netplan apply
    ```

## Ubuntu设置时区

1. 运行如下命令选择时区
    亚洲 -> 中国 -> 北京
    ```shell
    sudo tzselect
    ```

2. 创建时区软连接
    ```shell
    sudo ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    ```





## Ubuntu 设置 ssh 可通过 Root 用户登录

当我们使用远程连接工具通过 root 用户登录 Ubuntu 时，会出现 `Permission denied,please try again` 并一直让我们输入密码，出现这中情况，是因为我们系统默认禁止 root 用户 ssh 登录。解决办法如下

* 参考：[CSDN](https://blog.csdn.net/u010867294/article/details/78109551)

1. 普通用户登录，并切换为 root 用户

2. 更改 `sshd_config` 文件

   ```shell
   vim /etc/ssh/sshd_config
   ```

   找到 **#Authentication:**下的 `PermitRootLogin without-password` 如果是没注释的则注释掉。并在下面加上一行

   ```shell
   PermitRootLogin yes
   ```

   找错 **Authentication**下的 `PasswordAuthentication` 开启密码验证

   ```shell
   # Authentication:
    PasswordAuthentication yes //默认为no，改为yes开启密码登陆
   ```

3. 重启 ssh 服务，使配置生效

   ```shell
   /etc/init.d/ssh restart
   ```

   