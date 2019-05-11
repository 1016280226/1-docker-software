# 笔记四 Docker 搭建Jeknis + SVN 实现自动化部署

## 1. 基于Docker安装Jenkins环境
```shell
docker run --name devops-jenkins --user=root -p 8080:8080 -p 50000:50000 -v /opt/data/jenkins_home:/var/jenkins_home -d jenkins/jenkins:lts
```

## 2. 打开浏览器,访问地址 http://ip:8080
> *注意: 第一次启动的时候正在加载大概会等待3~10分钟*
![image](6FCE5A7AFF5F4FF9B0DEBCDBC9347A2F)

## 3. 解锁Jenkins 
![image](BBBB8D307B594A898DD31681D670EA0B)
进入jenkins容器,查看初始化容器密码
```shell
docker container exec -it container_id /bin/bash 

cat  /var/jenkins_home/secrets/initialAdminPassword
```

## 4. 选择安装推荐的插件
![image](E71735BB961C4F40AB7B5EEDB15FC634)
![image](3ABD743B755D499E951D6C78DD294668)

> *安装插件大概需要等待3-10分钟*

## 5.创建新的用户
![image](5732F2FA01DA4254A3362BCABD461AC9)
![image](F748421F67D6421EB2AA5CD88450328F)

## 6. Jenkins全局工具配置 
进入到jenkins容器中 echo $JAVA_HOME 获取java环境安装地址

### 6.1 JDK环境安装
在docker 镜像容器中，使用命令查看JAVA_HOME 获取java环境安装地址
```shell
echo $JAVA_HOME 
```
![image](4DDB49694AE64071B145D6908B685011)

### 6.2 Maven环境安装
![image](48400FDE2099477DAEE7BB22F94E7C12)

### 6.3 安装Jenkins 对应Maven 插件
找到 “系统管理“ - “安装插件” ，点击 “可选插件”，找到如下maven插件的版本  
插件名称 Maven Integration。
![image](E160F9D9E4024512857C12BE55E6093F)
![image](548C1E4FB0B7424EB9F7D2BBAEA99F8C)

## 7. Jenkins实现SpringBoot项目自动部署

### 7.1 新建一个发布maven 项目任务
![image](03F56B1453E949CD85CFC6C2A18EA7E5)

### 7.2 配置使用的是SVN 或者是Git,该地方使用的是SVN
![image](69EB6B02913145B6A93EC00FCD091DC9)

### 7.3 将maven 进行打包
```shell
clean install -Dmaven.test.skip=true
```
![image](F1D3AC60718F4C9B9D7C861E632201CE)

![image](54F8D3C70146455ABBEE35D0F77E03A6)

### 7.5 Jenkins启动成功之后执行shll脚本
```shell
#!/bin/bash
#服务名称
SERVER_NAME=App-Noise
# 源jar路径,mvn打包完成之后，target目录下的jar包名称，也可选择成为war包，war包可移动到Tomcat的webapps目录下运行，这里使用jar包，用java -jar 命令执行  
JAR_NAME=noise-service-impl-0.0.1-SNAPSHOT
# 源jar路径  
#/usr/local/jenkins_home/workspace--->jenkins 工作目录
#demo 项目目录
#target 打包生成jar包的目录
JAR_PATH=/var/jenkins_home/workspace/SmartCity/noise-service-impl/target
# 打包完成之后，把jar包移动到运行jar包的目录--->work_daemon，work_daemon这个目录需要自己提前创建
JAR_WORK_PATH=/var/jenkins_home/workspace/SmartCity/noise-service-impl/target



echo "查询进程id-->$SERVER_NAME"
PID=`ps -ef | grep "$SERVER_NAME" | awk '{print $2}'`
echo "得到进程ID：$PID"
echo "结束进程"
for id in $PID
do
	kill -9 $id  
	echo "killed $id"  
done
echo "结束进程完成"

#复制jar包到执行目录
echo "复制jar包到执行目录:cp $JAR_PATH/$JAR_NAME.jar $JAR_WORK_PATH"
cp $JAR_PATH/$JAR_NAME.jar $JAR_WORK_PATH
echo "复制jar包完成"
cd $JAR_WORK_PATH
#修改文件权限
chmod 755 $JAR_NAME.jar 
#在后台运行
BUILD_ID=dontKillMe nohup java -jar -Denv=DEV $JAR_NAME.jar & 
```
![image](20DB97E0179E400DBD07870F3EB9197D)

## 8. 点击保存应用
![image](FC3534133281421E8FBF2B1EB9858BFC)

## 9. 最后点击立即构建
![image](0C8651CBB62C41559D3B1A2B3012DD10)

> *第一次构建可能耗时比较长，因为需要下载一些相关依赖jar包*

## 10. 启动项目的容器端口映射出来（可以提前进行映射）
1. 重启容器
```shell
systemctl restart docker
```
2. 删除当前运行的容器
```shell
docker rm -f container_id
```

3. 重新执行端口映射
```shell
docker run --name devops-jenkins --user=root -p 8080:8080 -p 50000:50000 -v /opt/data/jenkins_home:/var/jenkins_home -d jenkins/jenkins:lts
```

## 11. 启动构建成功
![image](CDFEE0073C3148D48DE5698C60D89B0D)
