# 笔记一 Docker

[TOC]

## 1.Docker 简介

- Docker是基于Go语言实现的云开源项目，诞生于2013年初，最初发起者是dotClouw公司。

- Docker 自开源后受到广泛的关注和讨论，目前已有多个相关项目，逐断形成了围Docker的生态体系。dotCloud 公司后来也改名为Docker Ine。

- **Docker**是一个开源的**容器引擎**，**它有助于更快地交付应用**。
- **Docker**可将**应用程序**和**基础设施层 隔离**，并且能将基础设施当作程序一样进行管理。
- 使用 **Docker**可更**快地打包**、**测试**以及**部署应用程序**，并可以**缩短从编写**到**部署运行代码的周期**。

> - [Docker官方网址](https://docs.docker.com/)  
>
> - [Docker中文网址](http://www.docker.org.cn) 

![1553697942601](C:\Users\Calvin\AppData\Roaming\Typora\typora-user-images\1553697942601.png)



## 2.为什么要使用Docker

- **Docker**作为一种**轻量级的虚拟化**方式，Docker在运行应用上跟传统的虚拟机方式相比具有显著优势：

- **Docker**容器很快，启动和停止可以在**秒级实现**，这相比传统的虚拟机方式要快得多。

- **Docker**容器对**系统资源需求很少**，一台主机上可以**同时运行数千个Docker容器**。

- **Docker**通过类似Git的操作来方便用户获取、分发和更新应用镜像，指令简明，学习成本较低。

- **Docker**通过**Dockerfile配置文件来支持灵活的自动化创建和部署机制，提高工作效率。**

  ![1553698336987](C:\Users\Calvin\AppData\Roaming\Typora\typora-user-images\1553698336987.png)

### 2.1 简化程序

Docker 让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，便可以实现虚拟化。Docker改变了虚拟化的方式，使开发者可以直接将自己的成果放入Docker中进行管理。方便快捷已经是 Docker的最大优势，过去需要用数天乃至数周的 任务，在Docker容器的处理下，只需要数秒就能完成。

### 2.2 避免选择恐惧症

如果你有选择恐惧症，还是资深患者。Docker 帮你 打包你的纠结！比如 Docker 镜像；Docker 镜像中包含了运行环境和配置，所以 Docker 可以简化部署多种应用实例工作。比如 Web 应用、后台应用、数据库应用、大数据应用比如 Hadoop 集群、消息队列等等都可以打包成一个镜像部署。

### 2.3 节省开支

一方面，云计算时代到来，使开发者不必为了追求效果而配置高额的硬件，Docker 改变了高性能必然高价格的思维定势。Docker 与云的结合，让云空间得到更充分的利用。不仅解决了硬件管理的问题，也改变了虚拟化的方式。



## 3.Docker 原理架构

![1553697497781](C:\Users\Calvin\AppData\Roaming\Typora\typora-user-images\1553697497781.png)

a. **Docker daemon**（ Docker守护进程）

> Docker daemon是一个运行在**宿主机**（ **DOCKER-HOST**）的后台进程。
>
> 作用：可通过 **Docker客户端与之通信**。

 

b. **Client**（ Docker客户端）

> **Docker客户端是 Docker的用户界面**，它可以接受用户命令和配置标识，并与 Docker daemon通信。
>
> 图中， docker build等都是 Docker的相关命令。

 

c. **Images**（ Docker镜像）

> Docker镜像是一个只读模板，它包含创建 Docker容器的说明。
>
> 它和系统安装光盘有点像，使用系统安装光盘可以安装系统，同理，使用Docker镜像可以运行 Docker镜像中的程序。

 

d. **Container**（容器）

> 容器是镜像的**可运行实例**。
>
> 镜像和容器的关系有点类似于面向对象中，类和对象的关系。
>
> 可通过 Docker API或者 CLI命令来启停、移动、删除容器。



f. 运行原理：

> **容器→镜像→仓库**



## 4.Linux 环境上安装Docker

### 4.1 Docker 安装

1. **Docker** 是一个**开源的商业产品**，有两个版本：

> - **社区版**（Community Edition，缩写为 CE）
>
> - **企业版**（Enterprise Edition，缩写为 EE）。企业版包含了一些收费服务，个人开发者一般用不到



2. **Docker** 要求 CentOS 系统的内核版本在 **3.10以上** ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。

   a. 通过 **uname -r** 命令查看你当前的内核版本

   ```bash
   uname -r  
   ```

   b. 使用 root 权限登录 Centos。确保 yum 包更新到最新。

   ```bash
    yum -y update   
   ```

   c. 卸载旧版本(如果安装过旧版本的话)。

   ```bash
    yum remove docker docker-common   docker-selinux docker-engine   
   ```

   d. 安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的。

   ```bash
    yum install -y yum-utils   device-mapper-persistent-data lvm2   
   ```

   e. 设置yum源。

   ```bash
    yum-config-manager --add-repo   https://download.docker.com/linux/centos/docker-ce.repo   
   ```

   f. 可以查看所有仓库中所有docker版本，并选择特定版本安装。
   ```bash
   yum list docker-ce --showduplicates | sort -r   
   ```

   g.  安装docker。

   ```bash
   sudo yum install -y docker-ce     #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版18.03.1   
   ```

   h.  启动并加入开机启动。

   ```bash
   systemctl start docker   
   systemctl enable docker   
   ```

   i. 验证安装是否成功(有client和service两部分表示docker安装启动都成功了)。

   ```bash
   docker version   
   ```



### 4.2  配置镜像加速器

> [阿里云镜像加速器地址](https://cr.console.aliyun.com/cn-hangzhou/mirrors)

针对Docker客户端版本大于 1.10.0 的用户

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://m0p90m3l.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



## 5.Docker 常用的命令

1. 查看镜像文件。

   ```bash
   docker images
   ```

2. 删除镜像文件（根据镜像文件IMAGE ID，来删除镜像文件）。

   > -f  强制

   ```bash
   docker rmi -f IMAGE_ID
   ```

3. 查看运行中的容器。

   > -a 所有

   ```bash
   docker ps 
   ```

4. 查看容器中的的信息

   ```bash
   docker inspect CONTAINER_ID
   ```

5. 删除容器

   ```bash
   docker rm -f
   ```

6. 查看 安装软件 版本

   ```bash
   docker search install_softe_name 
   ```

7. 下载软件镜像

   > : 后指定版本号

   ```bash
   docker pull install_softe_name:version
   ```

8. 启动容器

   - 方法一：安装并且启动运行

     > -d 后台运行
     >
     > -p 宿主机端口 : 容器端口  #开放容器端口到宿主机端口

     ```bash
     docker run -d -p 81:80 install_softe_name
     ```

     > *提醒 ：使用 docker run命令创建容器时，会先检查本地是否存在指定镜像。如果本地不存在该名称的镜像， Docker就会自动从 Docker Hub下载镜像并启动一个 Docker容器。*

   - 方法二：启动运行

     ```bash
     docker start container_name #容器名称或容器ID
     ```

9. 进入容器目录

   ```bash
   docker container exec -it CONTAINER_ID /bin/bash 
   ```

   

## 6.使用Docker 安装Java8

1. 下载Java 8 镜像（安装包）

   ```bash
   docker pull install java:8
   ```



## 7.使用Docker 安装Nginx

1. 下载 **Nginx** 镜像文件

   ```bash
   docker pull nginx
   ```

2. 查看是否存在该镜像文件

   ```bash
   docker images
   ```

3. 为了能够对nginx 配置文件进行修改，需要进行**挂载**。

   - 创建挂载目录。

     ```bash
     mkdir -p /data/nginx/conf
     mkdir -p /data/nginx/conf.d
     mkdir -p /data/nginx/html
     mkdir -p /data/nginx/logs
     ```

   - 创建 /data/nginx/conf/nginx.conf。

     ```bash
     touch /data/nginx/conf/nginx.conf
     ```

   - 编辑nginx.conf 配置文件，编辑完，保存退出。

     ```shell
     user  nginx;
     worker_processes  1;
     
     error_log  /var/log/nginx/error.log warn;
     pid        /var/run/nginx.pid;
     
     
     events {
         worker_connections  1024;
     }
     
     
     http {
         include       /etc/nginx/mime.types;
         default_type  application/octet-stream;
     
         log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                           '$status $body_bytes_sent "$http_referer" '
                           '"$http_user_agent" "$http_x_forwarded_for"';
     
         access_log  /var/log/nginx/access.log  main;
     
         sendfile        on;
         #tcp_nopush     on;
     
         keepalive_timeout  65;
     
         #gzip  on;
     
         include /etc/nginx/conf.d/*.conf;
     }
     
     :wq! #保存退出
     ```

4. 启动容器，并且进行挂载。

   > -v  容器挂载外部配置文件 (使用挂载方式，外部的配置文件覆盖内部容器配置文件)

   ```bash
   docker run –name nginx -d -p 80:80  -v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf  -v /data/nginx/logs:/var/log/nginx -d docker.io/nginx
   ```

5. 在防火墙iptables 配置文件中或者使用命令，添加允许访问的端口号。

   ```bash
   # 关闭firewalld
   systemctl stop firewalld   
   systemctl mask firewalld
   
   #开放80端口(HTTPS)
   iptables -A INPUT -p tcp --dport 80 -j ACCEPT
   
   #保存上述规则
   service iptables save
    
   #开启服务
   systemctl restart iptables.service
   ```



## 8.使用Docker 安装MySQL

1. 查询mysql版本。

   ```bash
   docker search mysql
   ```

2. 下载MySQL5.7版本。

   ```bash
   docker pull mysql:5.7  (这里选择的是第一个mysql镜像， :5.7选择的5.7版本)   
   ```

3. 创建MySQL容器。

   > --name 指定容器的名称

   ```bash
   docker create --name mysql3308 -e MYSQL_ROOT_PASSWORD=root -p 3308:3306 mysql:5.7   
   ```

4. 启动容器

   ```bash
   docker start mysql3308 
   ```

5. 进入到容器

   ```bash
   docker exec -it mysql3308 bash
   ```

6. mysql连接

   ```bash
   mysql -uroot –p
   ```

7. 在防火墙iptables 配置文件中或者使用命令，添加允许访问的端口号。

   ```bash
   # 关闭firewalld
   systemctl stop firewalld   
   systemctl mask firewalld
   
   #开放80端口(HTTPS)
   iptables -A INPUT -p tcp --dport 80 -j ACCEPT
   
   #保存上述规则
   service iptables save
    
   #开启服务
   systemctl restart iptables.service
   ```



## 9.基于Docker 部署SpringBoot 项目

1. 将 jar 包上传到Linux 服务器 /usr/local/docker/app 目录下，在 jar 包所在目录创建名为Dockerfile 的文件。

2. 在Dockerfile 中添加以下内容：

   ```shell
   ###指定java8环境镜像
   FROM java:8
   ###复制文件到容器app-springboot
   ADD docker-spring-boot-swagger-1.0.jar /spring-boot-swagger-1.0.jar
   ###声明启动端口号
   EXPOSE 8080
   ###配置容器启动后执行的命令
   ENTRYPOINT ["java","-jar","/spring-boot-swagger-1.0.jar"]
   ```

3. 使用docker build 命令构建镜像。

   > 格式：docker build -t 镜像名称:标签 Dockerfile的相对位置

   ```bash
   docker build -t docker-spring-boot-swagger-1.0 ./
   ```

4. 启动 docker-spring-boot-swagger-1.0.jar 镜像文件

   ```bash
   docker run -p 8080:8080 docker-spring-boot-swagger-1.0
   ```

5. 在防火墙iptables 配置文件中或者使用命令，添加允许访问的端口号。

   ```bash
   # 关闭firewalld
   systemctl stop firewalld   
   systemctl mask firewalld
   
   #开放80端口(HTTPS)
   iptables -A INPUT -p tcp --dport 80 -j ACCEPT
   
   #保存上述规则
   service iptables save
    
   #开启服务
   systemctl restart iptables.service
   ```

   

