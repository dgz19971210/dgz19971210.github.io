---
title: Docker 概述
date: 2019-03-14 20:46:13
tags:
    - Docker
categories: Docker
encrypt:
description:
---

{% cq %} 前言 {% endcq %}

{% note info %}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;关于 Docker、Dockerfile、docker-compose 简单概述
{% endnote %}

<!-- more -->

# Docker

## 简介

**Docker 是一种容器技术，作用是用来快速部署服务**



## Docker 解决了什么问题

* **Docker解决了运行环境和配置问题，方便发布，也就方便做持续集成**。
* **更轻量的虚拟化，节省了虚拟机的性能损耗**





## Docker 应用场景

- 程序分发，gitlab的安装很恶心吧，所以有人做了gitlab的image
- 部署发布，可以很好的解决环境和配置问题
- PaaS，tsuru、flynn都是基于Docker的，CloudFoundry也要从warden迁移到Docker，不解释



## 容器的启动过程

1. docker client(即：docker终端命令行)会调用docker daemon请求启动一个容器，
2. docker daemon会向host os(即：linux)请求创建容器
3. linux会创建一个空的容器（可以简单理解为：一个未安装操作系统的裸机，只有虚拟出来的CPU、内存等硬件资源）
4. docker daemon请检查本机是否存在docker镜像文件（可以简单理解为操作系统安装光盘），如果有，则加载到容器中（即：光盘插入裸机，准备安装操作系统）
5. 将镜像文件加载到容器中（即：裸机上安装好了操作系统，不再是裸机状态）



## Docker 安装和基础命令

* 请阅读：[Ubuntu 安装及使用 Docker](https://dgzd.github.io/Ubuntu%E5%AE%89%E8%A3%85Docker.html#more)



# Dockerfile

## 简介

Dockerfile是一个文本文件，其内包含了一条条的指令，每一条指令构建一层，因此没一条指令的内容，就是描述该层应当如何构建

## 命令详解

### FROM

**指定基础镜像**。必备指令，必须是 Dockerfile 文件的第一条指令



### RUN

**执行命令**。命令有两种书写格式

1. shell 格式：`RUN` 后直接书写命令，就像在 shell 中一样

   ```dockerfile
   RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
   ```

2. exec 格式：`RUN ["可执行文件", "参数1", "参数2"]`

   ```dockerfile
   RUN ["chmod","+x","start.sh"]
   ```

   

### COPY

**复制文件**。把文件从本机 `上下文目录中` 复制到容器中，和 `RUN`指令 一样有两种格式

```dockerfile
# 格式1
COPY package.json /usr/src/app/

# 格式2
COPY ["package.json","/usr/src/app/"]
```



### ADD

**更高级的复制命令**。和 `COPY` 的使用格式一致，但在其基础上加了一些功能

1. 源路径可以是 URL，Docker 引擎会去下载链接文件放到目标路径，并设置权限为 `600` ，不如直接使用 `RUN` 指令配合 `wget` 或 `curl`，所以**不推荐使用** 
2. 如果 `<源路径>` 为一个 `tar` 压缩文件的话，压缩格式为 `gzip`, `bzip2` 以及 `xz` 的情况下，`ADD` 指令将会自动解压缩这个压缩文件到 `<目标路径>` 去。



### CMD

**容器启动时运行命令**。命令格式和 `RUN` 相似，在容器启动是会执行 CMD 所指定的命令



### ENTRYPOINT

**指定容器启动时要运行的命令**。目的和 `CMD` 一样，格式也是 `RUN` 指令的格式

但当指定了 `ENTRYPOINT` 后，`CMD` 的含义就发生了改变，不再是直接的运行其命令，而是将 `CMD` 的内容作为参数传给 `ENTRYPOINT` 指令，换句话说实际执行时，将变为：

```dockerfile
<ENTRYPOINT> "<CMD>"
```



### ENV

**设置环境变量**。在 Dockerfile 文件中设置环境变量，让下面的内容可以引用

无论是后面的其它指令，如 `RUN`，还是运行时的应用，都可以直接使用这里定义的环境变量。

例如：

```dockerfile
ENV NODE_VERSION 7.2.0

# 可多次引用上面所定义的环境变量
RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" 
```



### ARG

**构建参数**。效果和 `ENV` 一样，都是设置环境变量

所不同的是，`ARG` 所设置的构建环境的环境变量，在将来容器运行时是不会存在这些环境变量的。但是不要因此就使用 `ARG` 保存密码之类的信息，因为 `docker history` 还是可以看到所有值的。



### VOLUME

**定义匿名卷**。把容器中的目录挂载出来作为匿名卷，当向容器中写入数据时，不会写入容器中，会写入在本地的卷中

格式：

```dockerfile
# 把某一个目录挂载为匿名卷
VOLUME <路径>

#挂载多个目录为匿名卷
VOLUME ["<路径1>", "<路径2>"...]
```

容器运行时应该尽量保持容器存储层不发生写操作，对于数据库类需要保存动态数据的应用，其数据库文件应该保存于卷(volume)中。为了防止运行时用户忘记将动态文件所保存目录挂载为卷，在 `Dockerfile` 中，我们可以事先指定某些目录挂载为匿名卷，这样在运行时如果用户不指定挂载，其应用也可以正常运行，不会向容器存储层写入大量数据。

```dockerfile
VOLUME /data
```

这里的 `/data` 目录就会在运行时自动挂载为匿名卷，任何向 `/data` 中写入的信息都不会记录进容器存储层，从而保证了容器存储层的无状态化。当然，运行时可以覆盖这个挂载设置。比如：

```dockerfile
docker run -d -v mydata:/data xxxx
```

在这行命令中，就使用了 `mydata` 这个命名卷挂载到了 `/data` 这个位置，替代了 `Dockerfile` 中定义的匿名卷的挂载配置。



### EXPOSE

**暴露端口**。声明运行时容器提供服务端口，这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务

在 Dockerfile 中写入这样的声明有两个好处，一个是帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射；另一个用处则是在运行时使用随机端口映射时，也就是 `docker run -P`时，会自动随机映射 `EXPOSE` 的端口。

格式：

```dockerfile
EXPOSE <端口1> [<端口2>...]
```



### WORKDIR

**指定工作目录**。指定工作目录（或者称为当前目录）以后，各层的当前目录就被改为指定的目录，如该目录不存在，`WORKDIR` 会帮你建立目录。



### USER

**指定当前用户**。和 `WORKDIR` 相似，都是改变环境状态并影响以后的层。`WORKDIR` 是改变工作目录，`USER`则是改变之后层的执行 `RUN`, `CMD` 以及 `ENTRYPOINT` 这类命令的身份。

格式：

```dockerfile
USER <用户名>[:<用户组>]
```



### HEALTHCHECK

**健康检查**。

格式：

```dockerfile
HEALTHCHECK [选项] CMD <命令> # 设置检查容器健康状况的命令

HEALTHCHECK NONE    # 如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令
```



### ONBUILD

**为他人做嫁衣**。

`ONBUILD` 是一个特殊的指令，它后面跟的是其它指令，比如 `RUN`, `COPY` 等，而这些指令，在当前镜像构建时并不会被执行。只有当以当前镜像为基础镜像，去构建下一级镜像的时候才会被执行。



# Docker Compose

## 简介

Docker Compose 是用来对 docker 容器编排，把所有繁复的 docker 操作全都一条命令，自动化的完成。



## 安装

在 Linux 上安装十分简单。从 [官方 GitHub Release](https://github.com/docker/compose/releases) 处直接下载编译好的二进制文件即可。

```shell
curl -L https://github.com/docker/compose/releases/download/1.24.0-rc1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```



## 卸载

如果是二进制包安装的，删除二进制包即可

```shell
$ sudo rm /usr/local/bin/docker-compose
```



## Compose 模板文件

MySQL + Nginx + Tomcat 的商城项目

```yaml
version: '3'
services:
  tomcat:
    restart: always
    build:
      context: ./bookstore_admin
      dockerfile: tomcat_dockerfile
    image: tomcat
    container_name: tomcat
    ports:
      - 8080:8080
    networks:
      - my_net
    environment:
      TZ: Asia/Shanghai

  nginx:
    restart: always
    image: nginx
    container_name: nginx
    ports:
      - 80:80
    networks:
       - my_net
    volumes:
      - ./conf/nginx.conf:/etc/nginx/nginx.conf
      - ./wwwroot:/usr/share/nginx/wwwroot

  db:
    restart: always
    build:
      context: ./bookstore_db
      dockerfile: Dockerfile
    container_name: mysql
    ports:
      - 3306:3306
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: root
    command:
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
      --max_allowed_packet=128M
      --sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO"
    volumes:
      - ./data/mysql:/var/lib/mysql
    networks:
      - my_net

networks:
  my_net:
    driver: bridge
    ipam:
      config:
        - subnet: 172.30.0.1/16

```



## Docker Compose常用命令

### build

构建（重新构建）项目中的服务容器。

```dockerfile
docker-compose build -f yml文件
```

### up

构建镜像，（重新）创建服务，启动服务，并关联服务相关容器的一系列操作。

```dockerfile
docker-compose -f yml文件 up 

# -d 以守护进程运行
```

### down

停止up 命令启动的容器并

```dockerfile
docker-compose -f xx.yml down
```

### logs

查看服务容器的输出。

```dockerfile
docker-compose logs [options] [SERVICE...]
```

### rm

删除所有（停止状态的）服务容器。推荐先执行 `docker-compose stop` 命令来停止容器。

```dockerfile
docker-compose rm [options] [SERVICE...]

# -f, --force 强制直接删除，包括非停止状态的容器。一般尽量不要使用该选项。
# -v  删除容器所挂载的数据卷
```



### 其他命令

请查看 [Docker从入门到实战](https://yeasy.gitbooks.io/docker_practice/compose/commands.html) 



# Docker 网络设置

## 查看 docker 的网卡

```dockerfile
$ docker network ls
```

Docker安装后，默认会创建下面三种网络类型

1. **bridge：桥接网络**

   默认情况下启动的 Docker 容器，都是使用 bridge，Docker安装时创建的桥接网络，每次Docker容器重启时，会按照顺序获取对应的IP地址，这个就导致重启下，Docker 的 IP 地址就变了



2. **host：主机网络**

   使用 `--network=host` ，此时，Docker 容器的网络会附属在主机上，两者是互通的。
   例如，在容器中运行一个Web服务，监听8080端口，则主机的8080端口就会自动映射到容器中。



3. **none：无指定网络**

   使用 `--network=none` ，docker 容器就不会分配局域网的IP



## 启动 Docker时指定网络类型

在 docker 容器启动时，把容器加入指定的网络，使用 `--network 网络类型`

```dockerfile
docker run -itd --name test1 --network bridge --ip 172.17.0.10 centos:latest /bin/bash
```



## 创建自定义网络：（为容器设置固定IP）

启动 Docker 容器的时候，使用默认的网络是不支持指派固定IP的，如下

```dockerfile
docker run -itd --net bridge --ip 172.17.0.10 centos:latest /bin/bash
```

会出现错误提示：

`docker: Error response from daemon: User specified IP address is supported on user defined networks only.`



**解决方式如下**

* 创建自定义网络

  ```dockerfile
  docker network create --subnet=172.18.0.0/16 mynetwork
  ```

  

* 创建 Docker 容器

  ```dockerfile
  docker run -itd --name networkTest1 --net mynetwork --ip 172.18.0.2 centos:latest /bin/bash
  ```



注：当使用 docker-conpose 文件时，可以在 compose 文件中设置容器的网络





# 参考

* [neptune 破壁人](https://www.cnblogs.com/neptunemoon/p/6512121.html#toc_2)
* [雪之谷](https://www.cnblogs.com/xuezhigu/p/8257129.html)
* [Docker从入门到实战](https://yeasy.gitbooks.io/docker_practice/image/build.html)