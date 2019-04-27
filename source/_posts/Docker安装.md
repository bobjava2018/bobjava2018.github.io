---
title: Docker安装
date: 2019-04-27 15:51:34
tags:
---

#                                 Docker

## 一、简介

  Docker是一个开源的应用容器引擎

  将软件编译成一个镜像；然后再镜像里面各种软件做好配置，将镜像发布出去，在服务器上就可以直接使用这个镜像来运行各种软件。运行中的这个镜像叫做容器，容器启动速度快，类似ghost操作系统，安装好了就什么都有了。

## 二、 Docker的核心概念

docker主机（host）：安装了Docker程序的机器（Docker是直接安装在操作系统上的）

docker客户端（Client）：操作docker的主机

docker仓库（Registry):用来保存打包好的软件镜像

docker镜像（image）:打包好的镜像，一般都从docker仓库中下载

docker容器（Container）:镜像启动 后的实力（启动一个mysql镜像5次，就会有5个容器）

docker镜像的使用步骤：

1. 安装Docker
2. 去Docker仓库找到这个软件对应的镜像
3. 使用Docker运行这个镜像，镜像就会生成一个容器
4. 对容器的启动停止，就是对软件的启动和停止

## 三、Doker的安装

1. 安装linux系统

2. 在linux上安装docker

   ````she
   1、查看centos版本,内核3.1以上YUM 
   # uname -r
   3.10.0-693.el7.x86_64
   要求：大于3.10
   如果小于的话升级*（选做）
   # yum update
   2、安装docker
   # yum install docker
   3、启动docker
   # systemctl start docker
   # docker -v
   4、开机启动docker
   # systemctl enable docker
   5、停止docker
   # systemctl stop docker
   ````

   

## 四、Docker 的常用操作

### 1、镜像操作

1. 搜索

   ```shell
   docker search mysql
   ```

   默认会去docker hub网站查找

   ![1555837998110](Docker安装\1555837998110.png)

      我们也可以手动去docker hub查找 <https://hub.docker.com/>

     ![1555838069439](Docker安装\1555838069439.png)

2、拉取镜像ags标签进入 里面， 里面的 rc  10.4是标签，一会儿下载要用

  ``` shell
默认最新版本  如果后面没有冒号标签 默认安装最新版本 latest
# docekr pull mysql
安装指定版本  
# docker pull mariadb:10.4

//版本太低的话需要升级
# rpm -qa | grep docker – – 列出包含docker字段的软件的信息
# yum remove docker-1.13.1-53.git774336d.el7.centos.x86_64 
# yum remove docker-client-1.13.1-53.git774336d.el7.centos.x86_64 
# yum remove docker-common-1.13.1-53.git774336d.el7.centos.x86_64
使用curl升级到最新版 
# curl -fsSL https://get.docker.com/ | sh

天下容器, 唯快不破  (配置此镜像加速的前置条件，是将docker 升级到1.8以上)
Docker Hub 提供众多镜像，你可以从中自由下载数十万计的免费应用镜像， 这些镜像作为 docker 生态圈的基石，是我们使用和学习 docker 不可或缺的资源。为了解决国内用户使用 Docker Hub 时遇到的稳定性及速度问题 DaoCloud 推出永久免费的新一代镜像站服务
  <https://www.daocloud.io/mirror#accelerator-doc/>
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io

  ```

3、查看下载的所有的镜像、删除

```SHE
# docker images  //查看镜像
# docker  rmi imageid  //删除的时候使用镜像ID
```

![1555841748124](Docker安装\1555841748124.png)

###    2、容器操作

```shell
1、搜索镜像
# docker search mariadb
2、启动容器  此处要注意端口映射  即要通过主机的端口去
虚拟机:3307
容器的:3308
-d:后台运行
-p:主机端口(所启动的容器在主机中所占的端口):容器内端口（容器内的镜像 在容器内的的端口，此端口一般是默认的比如mysql 3036   tomcat8080 ）

连接数据库：192.168.0.103:3037
#docker run --name mariadb01 -e MYSQL_ROOT_PASSWORD=1030406963 -d -p 13308:3306 mariadb:10.4
#docker run --name mariadb02 -e MYSQL_ROOT_PASSWORD=1030406963 -d -p 13309:3306 mariadb:10.4
此处用了两次命令分别在 主机的13309和13308端口启动两个数据库，我们可以在外面通过这两个端口来访问mariadb

//查看所有容器
# docker ps -a
//停止容器
# docker stop id
//删除容器
# docker rm id

如果远程连接不上数据库，可进行以下操作（一般情况下是配置好的）
//进入容器内 可以再次连接数据库
# docker exec -it id  /bin/bash
#mysql -uroot -p
#输入密码
#use mysql
#GRANT ALL PRIVILEGES ON *.* TO 'root' @'%' IDENTIFIED BY '1030406963' WITH GRANT OPTION;
#flush privileges;


3.安装tomcat
# docker pull tomcat:9.0.19-jre8
# docker run -it --rm -d -p 8888:8080 tomcat:9.0.19-jre8
```