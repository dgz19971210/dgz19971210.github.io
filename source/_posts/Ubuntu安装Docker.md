---
title: Ubuntu安装及使用Docker
date: 2019-03-01 15:06:59
tags: 
    - Ubuntu
    - Linux
    - Docker
categories: Docker
encrypt:
description:
---

{% cq %} 前言 {% endcq %}

{% note info %}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ubuntu 下安装及使用 Docker
{% endnote %}

<!-- more -->

## Docker

### 概念

Docker 是将发布程序运行所需要的环境打包到一起，自动化运行的容器；是一个开源项目，支持大部分的 Linux 发行版，操作系统层以上的虚拟化技术。容器是完全使用沙箱机制，相互之间不会有任何接口。

### 优点

与传统的虚拟化方式相比：

* 更高效的利用系统资源
* 更快的启动速度
* 一致的运行环境
* 持续交互和部署
* 更轻松的迁移
* 更轻松的维护和扩展

### 组成

* Docker Client：通过 Api 访问 Docker Daemon 管理 Docker 镜像
* Docker Daemon：守护进程，负责 Docker 镜像的创建、删除、启动、停止等服务
* Docker Image：镜像，一张 “只读” 的系统 CD
* Docker Container：Docker 的容器，Docker Images 运行实例
* Docker Registry：Docker Images 的仓库，Docker Hub：https://www.dockerhub.com



## Ubuntu 安装 Docker
参考：[简书](https://www.jianshu.com/p/07e405c01880)

### 方式一

* 使用官方脚本自动安装

  ```shell
  curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
  ```



###  方式二

* 添加HTTPS协议，允许apt从HTTPS安装软件包

  ```shell
  sudo apt-get install  apt-transport-https  ca-certificates curl  software-properties-common
  
  ```

* 添加 Docker 公共密钥

  ```shell
  # docker 官方源
  curl -fsSL  https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add
  
  # docker 中科大源，添加中科大即可
  curl -fsSL  https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
  
  ```

* 设置版本库类型（Ubuntu18.04对应版本**“bionic“”**），软件版本包括三种：stable、edge、test

  ```shell
  # Docker 官方
  sudo add-apt-repository "deb [arch=amd64]  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" 
  
  
  # Docker 中科大
  sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable" 
  
  ```

* 安装 Docker CE

  ```shell
  # 更新 apt-get 源
  sudo apt-get update
  
  # 安装
  sudo apt-get install docker-ce
  ```

  



## Docker 常用命令

参考：[CSDN](https://blog.csdn.net/hcljava/article/details/78588623)

### 添加用户到 docker 用户组

1. 创建 docker 用户组

   ```shell
   sudo groupadd docker
   ```

   

2. 把需要的用户加入 docker 用户组

   ```shell
   sudo usermod -aG docker ${USER}
   
   sudo gpasswd -a dylan docker
   ```

   

3. 重启 docker 服务

   ```shell
   sudo systemctl restart docker
   
   sudo service docker restart
   ```



4. 切换用户后重新登录此普通用户，才能生效



### Docker 基本

* 启动 docker

  ```shell
  systemctl start docker
  ```

* 查看 docker 版本

  ```shell
  # 简单查看版本
  docker -v  [--version]
  
  # 查看详细版本
  docker version
  ```

* 显示 docker 系统的信息

  ```
  docker info
  ```



### 操纵 Docker 镜像

* 检索 image

  ```
  docker search image-name
  ```

  

* 下载 image

  ```
  docker pull image-name
  ```

  

* 列出镜像列表

  ```
  docker images
  ```

  

* 删除一个或多个 images 镜像（删除时需要先删除容器，使用 `docker ps -a` 查看运行的容器，使用 `docker rm 容器id` 删除容器） ——参考：[CSDN](https://blog.csdn.net/flydreamzhll/article/details/80900509)

  ```
  docker rmi image-name
  ```

  

* 显示一个镜像的历史

  ```
  docker history image-name
  ```

  

* 通过容器创建镜像

  从已经创建的容器中更新镜像，并且提交这个镜像 *使用 Dockerfile 指令来创建一个新的镜像 下面通过已存在的容器创建一个新的镜像

  ```
  docker commit -m="First Image" -a="keke" 7a15f99695c0 keke/unbantu:17.10.0
  ```

  参数说明：

  ```
  -m  提交的描述信息
  -a  指定镜像作者
  7a15f99695c0 记住这个是容器id，不是镜像id
  keke/unbantu:17.10.0 创建的目标镜像名
  ```

  

* 镜像发布

  1. 在[Docker](https://www.docker.com/) 注册账户，发布的镜像都在[这个页面里](https://cloud.docker.com/repository/list)展示

  2. 将上面做的镜像unbantu，起个新的名字unbantu-test

     ```
     docker tag keke/unbantu:17.10.0 keke/unbantu-test:lastest
     ```

  3. 登录 docker

     ```
     docker login
     ```

  4. 上传 Ubuntu 镜像

     ```
     docker push keke/unbantu-test:lastest
     ```

     

### 启动容器

Docker 容器可以理解为在沙盒中运行的进程。这个沙盒包含了该进程运行所必须的资源，包括文件系统、系统类库、shell 环境等等。但这个沙盒默认是不会运行任何程序的。你需要在沙盒中运行一个进程来启动某一个容器。这个进程是该容器的唯一进程，所以当该进程结束的时候，容器也会完全的停止。



* 在容器中安装新的程序

  ```
  docker run image-name apt-get install -y -name
  ```

  注意：在执行apt-get 命令的时候，要带上-y参数。如果不指定-y参数的话，apt-get命令会进入交互模式，需要用户输入命令来进行确认，但在docker环境中是无法响应这种交互的。apt-get 命令执行完毕之后，容器就会停止，但对容器的改动不会丢失.

  

* 在容器中运行库 echo 命令

  ```
  docker run image-name echo "hello word"
  ```

  

* 交互式进入容器中

  ```
  docker run -i -t image_name /bin/bash
  ```



### 查看容器

* 列出当前所有正在运行的 container

  ```
  docker ps
  ```

  

* 列出所有的 container

  ```
  docker ps -a
  ```

  

* 列出最近一次启动的 container

  ```
  docker ps -l
  ```





### 操纵容器

* 保存对容器的修改

  当你对某一个容器做了修改之后（通过在容器中运行某一个命令），可以把对容器的修改保存下来，这样下次可以从保存后的最新状态运行该容器

  ```
  docker commit ID new-image-name
  ```

  选项：` -a, --author="" Author; -m, --message="" Commit message`



* 删除所有容器

  ```
  docker rm `docker ps -a -q`
  ```



* 删除单个容器

  ```
  docker rm Name/ID
  ```

  -f, --force=false; -l, --link=false Remove the specified link and not the underlying container; -v, --volumes=false Remove the volumes associated to the container



* 停止、启动、杀死一个容器

  ```
  docker stop Name/ID  
  
  docker start Name/ID  
  
  docker kill Name/ID
  ```

  

* 从一个容器中取出日志

  ```
  docker logs Name/ID
  ```

  -f, --follow=false Follow log output; -t, --timestamps=false Show timestamps



* 列出一个容器里面被改变的文件或者目录，list列表会显示出三种事件，A 增加的，D 删除的，C 被改变的

  ```
  docker diff Name/ID
  ```



* 显示一个运行的容器里面的进程信息

  ```
  docker top Name/ID
  ```



* 从容器里面拷贝文件/目录到本地一个路径

  ```
  docker cp Name:/container-path to-path  
  
  docker cp ID:/container-path to-path
  ```

  

* .重启一个正在运行的容器

  -t, --time=10 Number of seconds to try to stop for before killing the container, Default=10

  ```
  docker restart Name/ID
  ```



* 在容器外，使用容器执行命令

  ```
  docker exec 容器名 命令
  
  docker exec -i -t  mynginx /bin/bash
  ```



* 附加到一个运行的容器上面

  ```
  docker attach ID #重新启动并运行一个交互式会话shell
  ```



### 构建镜像

#### 一、通过 commit 命令提交容器为镜像

在容器中设置好我们所需要的配置，使用 `docker commit` 来提交容器作为镜像 

```
docker commit -m="A new custom image" --author="Bourbon Tian" b437ffe4d630 test/apache2:webserver
```

选项：

* `-m` ：用来指定创建镜像要提交的消息
* `--author` ：用来列出该镜像的作者信息



#### 二、通过 Dockerfile 文件构建镜像

1. 创建文件夹并在其中创建 Dockerfile 文件

2. 在 Dockerfile 中写入镜像 有关指令

3. 运行 `docker build` 构建镜像

   ```
   docker build -t dylan/ubuntu:v1 . 
   ```

   选项：`-t` ：tag 、`-f`：dockerfile 文件位置

   ​	



### 保存和加载镜像

* 保存镜像到一个 tar 包； -o --output="" Write to an file

  ```
  docker save image-name -o file-path
  
  # Ubuntu 上保存nginx 到 tmp 下
  docker save nginx:latest > /tmp/nginx.tar
  ```

  

* 加载一个tar包格式的镜像; -i, --input="" Read from a tar archive file

  ```
  docker load -i file-path
  ```

  

* 从 A 机器拷贝到 B 机器

  ```
  docker load < /home/keke/main.tar
  ```

  