---
title: Ubuntu安装MySql
date: 2019-02-28 19:54:51
tags: 
    - Ubuntu
    - Linux
    - Java
categories: Linux
encrypt:
description:
---

{% cq %} 前言 {% endcq %}

{% note info %}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ubuntu安装MySql
{% endnote %}

<!-- more -->



# Ubuntu 安装 MySQL

## 安装

* 修改 apt 源（已修改的可忽略）

  1. 备份原文件

     ```shell
     sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
     ```

  2. 编辑源列表文件

     ```shell
     sudo vim /etc/apt/sources.list
     ```

  3. 删除原来的文件内容，添加下列源

     ```list
     deb http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
     deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
     deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
     deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
     deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
     deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
     deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
     deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
     deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
     deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
     ```

  4. 更新 apt-get 数据源

     ```shell
     sudo apt-get update
     ```

     

* 使用 apt-get 安装 `mysql-server`

  ```shell
  apt-get install mysql-server
  ```



## 配置

* 运行安全脚本

  ```shell
  mysql_secure_installation
  ```

  这将提示您输入您在之前步骤中创建的 root 密码。您可以按 Y，然后 ENTER 接受所有后续问题的默认值，但是要询问您是否要更改 root 密码。您只需在之前步骤中进行设置即可，因此无需现在更改。

* 查看 MySQL 状态

  ```shell
  systemctl status mysql.service
  ```

* 查看 MySQL 版本

  ```shell
  mysqladmin -p -u root version
  ```

* 配置远程访问

  ```shell
  # 修改配置文件
  vim /etc/mysql/mysql.conf.d/mysqld.cnf
  
  # 注释掉绑定的 IP地址
  bind-address = 127.0.0.1
  
  # 重启 MySQL
  service mysql restart
  
  ```

* 登录 MySQL，并在其中操作

  ```mysql
  -- 登录
  mysql -u root -p
  
  -- 设置密码安全策略
  set global validate_password_policy=0;
  -- 设置密码最少长度
  set global validate_password_length=1;
  
  -- 允许root用户/密码“123456”,在localhost发起的访问
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY '123456' WITH GRANT OPTION;
  -- 允许root用户/密码“123456”,在127.0.0.1发起的访问
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'127.0.0.1' IDENTIFIED BY '123456' WITH GRANT OPTION;
  -- 允许root用户/密码“123456”,在局域网所以ip发起的访问
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
  
  -- 刷新权限
  FLUSH PRIVILEGES;
  ```

  

* 修改 `mysqld.cnf` 配置文件

  ```shell
  # 进入文件
  vi /etc/mysql/mysql.conf.d/mysqld.cnf
  ```

  

  在 `[mysqld]` 节点上增加一个节点

  ```
  [client]
  default-character-set=utf8
  ```

  

  在 `[mysqld]` 节点底部增加如下配置

  ```
  skip-grant-tables
  default-storage-engine=INNODB
  character-set-server=utf8
  collation-server=utf8_general_ci
  lower-case-table-names = 1
  ```



​	注：出现拒绝访问root用户的解决方案，添加上述 `skip-grant-tables` 配置

​      参考：[CSDN](https://blog.csdn.net/qq_36675754/article/details/81381341)

​      配置文件详情：[mysql配置文件详情](https://blog.csdn.net/lienfeng6/article/details/78140404)



## 常用命令

- 启动

  ```shell
  # 启动方式1 
  service mysql start
  
  # 启动方式2
  systemctl start mysql.service
  ```

  

- 停止

  ```shell
  service mysql stop
  ```

  

- 重启

  ```shell
  service mysql restart
  ```

- 查看 MySQL 状态

  ```shell
  systemctl status mysql.service
  ```

  

## 部署应用到生产环境

* 导入数据库

  使用 MySQL 工具导入即可

  

  

* 设置 Tomcat 远程访问密码

  ```xml
  1. 修改 Tomcat 下的 conf/tomcat-users.xml
  <tomcat-users> 
  <role rolename="manager-gui"/> 
  <role rolename="admin-gui"/> 
  <role rolename="manager-script"/> 
  <user username="admin" password="admin" roles="manager-gui,admin-gui,manager-script"/> 
  </tomcat-users> 
  
  2. 同时还需要修改，如无新建conf/Catalina/localhost/manager.xml 内容如下：
  
  <Context privileged="true" antiResourceLocking="false"
           docBase="${catalina.home}/webapps/manager">
      <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
  </Context>
  ```



* 部署 WEB 项目

  1. 手动发布

     访问地址：`http://10.3.133.33:8080/manager` 页面，利用 Tomcat 进行发布

     

  2. Maven 插件发布

     在项目最顶层添加插件，运行插件即可

     ```xml
      <build>
             <plugins>
                 <!-- tomcat插件 -->
                 <plugin>
                     <groupId>org.apache.tomcat.maven</groupId>
                     <artifactId>tomcat7-maven-plugin</artifactId>
                     <version>2.2</version>
                     <configuration>
                         <url>http://10.3.50.119:8080/manager/text</url>
                         <username>admin</username>
                         <password>admin</password>
                         <update>true</update>
                         <path>/test</path>
                     </configuration>
                 </plugin>
             </plugins>
         </build>
     ```

     

