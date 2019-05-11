# 笔记三 Docker 搭建GitLab
[TOC]

## 1. 基于Docker部署GitLab环境搭建
> - *建议虚拟机内存2G以上*
> - *注意：一定要配置阿里云的加速镜像*


1. 下载镜像文件。
```shell
docker pull beginor/gitlab-ce:11.0.1-ce.0
```

2. 创建GitLab 的 **配置 (etc)** 、**日志 (log)** 、**数据 (data)** 放到容器之外， 便于日后升级， 因此请先准备这三个目录。
```shell
mkdir -p /mnt/gitlab/etc
mkdir -p /mnt/gitlab/log
mkdir -p /mnt/gitlab/data
```

3.	运行 GitLab容器。
```shell
docker run \
    --detach \
    --publish 8443:443 \
    --publish 8090:80 \
    --name gitlab \
    --restart unless-stopped \
    -v /mnt/gitlab/etc:/etc/gitlab \
    -v /mnt/gitlab/log:/var/log/gitlab \
    -v /mnt/gitlab/data:/var/opt/gitlab \
    beginor/gitlab-ce:11.0.1-ce.0 
```

4. 停止 docker 容器，并且删除。
```shell
docker stop container_id
docker rm container_id
```

5. 修改 **/mnt/gitlab/etc/gitlab.rb**, 把**external_url**改成部署机器的域名或者IP地址。
```shell
external_url 'http://192.168.212.227'
```

6. 修改 **/mnt/gitlab/data/gitlab-rails/etc/gitlab.yml** , 将host的值改成映射的外部主机ip地址和端口，这里会显示在gitlab克隆地址。

> 找到关键字 *## Web server settings*

![image](8ACA75FFD11A4CE48BCE712C034C2EC7)


7. 重新运行 GitLab 容器
```shell
docker run \
    --detach \
    --publish 8443:443 \
    --publish 8091:8091 \
    --name gitlab \
    --restart unless-stopped \
    -v /mnt/gitlab/etc:/etc/gitlab \
    -v /mnt/gitlab/log:/var/log/gitlab \
    -v /mnt/gitlab/data:/var/opt/gitlab \
    beginor/gitlab-ce:11.0.1-ce.0 
```

8.  浏览器打开 [http:ip:8090](https://note.youdao.com/)

> *默认账号和密码：root* 

![image](E804B3566AC14D8C959EFB5E1D436E25)


