---
title: Docker入门
tags: 
 - Docker
categories: 
 - Docker
---
[TOC]

## Docker

## 1.核心概念

**docker主机（Host）**：安装了doocker程序的机器（docker直接安装在操作系统之上）

**docker客户端（Client）**：连接docker主机进行操作

**docker仓库（Registry）**：用来保存各种打包好的软件镜像

**docker镜像（Image）**：软件打包好的镜像，放在docker仓库中

**docker容器（Container）**：镜像启动后的实例称为一个容器，容器是独立运行的一个或一组应用

## 2.准备工作：

更新内核和yum库

> yum update -y	(y代表yes，即是一切提示都确认通过)

1.查看系统内核版本（必须是3.10以上）

> uname -r

2.安装所需的依赖软件包

> yum install -y yum-utils device-mapper-persistent-data lvm2

 yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018112108522160.)
由于已经更新过内核和yum库，此处已经安装过最新版本，该操作是为了测试是否已经成功安装。
3.设置yum源

> yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

使用以上命令设置稳定存储库。 您始终需要稳定的存储库，即使您也想从边缘或测试存储库安装构建。
## 3.安装&启动docker
1.安装docker
> yum install -y docker

2.启动并设置为开机启动docker

> systemctl start docker
> systemctl enable docker

3.查看docker版本，确认是否安装成功

> docker --version

4.登录阿里云-》产品与服务-》容器镜像服务-》镜像加速器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181121092533992.?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5OTA1OTEw,size_16,color_FFFFFF,t_70)
按黑色部分配置或者

5.配置docker镜像加速器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181121093205146.)

6.停止docker

> systemctl stop docker

## 4.常用操作

1）镜像操作

| 操作 | 命令                   | 说明                                                    |
| ---- | ---------------------- | ------------------------------------------------------- |
| 检索 | docker search 关键字   | 去docker hub上检索镜像，如镜像的tag                     |
| 拉取 | docker pull 镜像名:tag | :tag是可选的，tag表示标签，多为软件的版本，默认是latest |
| 列表 | docker images          | 查看所有本地镜像                                        |
| 删除 | docker rmi image-id    | 删除指定的本地镜像                                      |

2）容器操作

| 操作                   | 命令                                                         | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 运行                   | docker run --name container-name -d image-name               | --name:自定义容器名<br />-d:后台运行<br />image-name:指定镜像模板 |
| 列表                   | docker ps（查看运行中的容器）                                | 加上-a可以查看所有容器                                       |
| 停止                   | docker stop container-name/container-id                      | 停止当前你运行的容器                                         |
| 启动                   | docker start container-name/container-id                     | 启动容器                                                     |
| 删除                   | ddocker rm container-id                                      | 删除指定容器                                                 |
| 端口映射               | -p 6379:6379<br />eg:docker run -d -p6379:6379 --name myredis docker:io/redis | -p:主机端口（映射到）容器内部的端口                          |
| 容器日志               | docker logs container-name/container-id                      |                                                              |
| 在运行的容器中执行命令 | docker exec -it containerId /bin/bash<br />进入后查看用cat<br />doker exec containerId 命令（eg:java -version） |                                                              |

端口测试时可能由于防火墙问题不能访问！！！

（本人在mysql和springboot两个容器通信时由于防火墙问题，访问不了）

报如下错误：

```java
Caused by: java.net.NoRouteToHostException: No route to host (Host unreachable)
    
```

> service firewalld status 查看防火墙状态
>
> service firewalld stop 关闭防火墙

3）辅助命令

`docker --help`

`docker -version`

`docker info`

查看容器日志，mysql容器创建时没有指定初始值密码会报如下错，此时查看容器日志可以分析得到

```:aland_islands:
[root@localhost local]# docker logs 84c3f5077053
error: database is uninitialized and password option is not specified
  You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD
```

`docker logs containerId`



```shell
安装jenkins时，由于挂载目录卷用户为root，而jenkins用户为jenkins会出现如下错误
Can not write to /var/jenkins_home/copy_reference_file.log. Wrong volume permissions?
touch: cannot touch ‘/var/jenkins_home/copy_reference_file.log’: Permission denied

解决方案：sudo chown -R 1000 绝对路径
```

## 5.在docker容器上安装*************等等

**下图ribbitmq错误，应该为rabbitmq！！！**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181121112218785.)
知识点总结：
**镜像和容器的关系，就像类和对象的关系类似**

1.查找

> docker search tomcat

2.从镜像仓库中拉取

> docker pull tomcat:8

3.查看已经获取的镜像

> docker images

4.列出所有正在运行的容器

> docker ps -a

-a :显示所有的容器，包括未运行的。

5.删除容器

> docker rm containerId

6.删除镜像

> docker rmi imageId

7.创建并运行容器
tomcat8:

> docker run -d --name tomcat1 -p 9000:8080 78b258e36eed

-d :后台运行容器
--name:容器名字
9000:通知服务器（虚拟机）的端口，对外暴露
8080:tomcat对外暴露的端口
78b258e36eed:tomcat镜像id

mysql:

> docker run -d --name mysql1 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=abc123 a876cc5d29e4  

-e:环境变量
MYSQL_ROOT_PASSWORD=abc123:密码
a876cc5d29e4 :镜像id

8.启动一个或多个已经被停止的容器

> docker start containerId

## 6.容器数据卷

**能干嘛**？

> 容器的持久化和容器之间继承+共享数据

**数据卷**

如何添加？

1.直接-v命令添加

> docker run -it -v /宿主机绝对路径目录:/容器内目录 镜像名

只读：

> docker run -it -v /宿主机绝对路径目录:/容器内目录:ro 镜像名     

和redis的rdp和aop类似，主机修改后，容器也会同步

可以用docker inspect命令查看是否挂载成功

2.使用Dockerfile

镜像的脚本文件，类似于linux的shell编程

> docker build -f 绝对路径 -t 镜像名称 .

**数据卷容器**

拓展：子容器继承父容器，数据共享

容器之间配置信息的传递，数据卷的生命周期一直持续到没有容器使用他为止

命名的容器挂载数据卷，其他容器通过挂载这个（父容器）实现数据共享，挂载数据卷的容器，称之为数据卷容器

> docker run -it --name dc02 --volumes-from zzyy/centos

## 7.构建镜像

1.通过docker commit命令   容器方式构建

2.通过docker build命令        Dockerfile文件方式构建

**大致流程**

1. docker从基础镜像运行一个容器（FROM）
2. 执行一条指令并对容器进行修改
3. 执行类似docker commit的操作提交一个新的镜像层（ADD）
4. docker再基于刚提交的镜像运行一个新容器
5. 执行dockerfile中的下一条指令直到所有指令都执行完成

Dockerfileh保留字指令

1）From：基础镜像，当前新镜像是基于哪个镜像的

> From<image>:<tag>	
>
> 必须是已经存在的镜像（基础镜像）
>
> 在Dockerfile中必须是第一条非注释的指令

2）MAINTAINER：镜像维护者的姓名和邮箱地址

3）RUN：容器构建时需要运行的命令

4）EXPORT：当前容器对外暴露出的端口

5）WORKDIR：指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点

6）ENV：用来在构建镜像过程中设置环境变量

7）ADD：拷贝+解压缩

8）COPY：拷贝

9）VOLUME：容器数据卷，用于数据保存和持久化

10）CMD：指定一个容器启动时要运行的命令，一个Dockerfile中可以有多个CMD命令，但只有最后一个会生效，CMD会被docker run之后的参数替换

11）ENTRYPOINT：指定一个容器启动时要运行的命令

12）ONBUILD：当构建一个被继承的Dokcerfile时运行命令，父镜像在被子继承后父镜像的onbuild被触发

新知识点：kubernetes