# 笔记二 Docker 搭建Maven 私服

[TOC]

## 1. Maven 私服

- a.**概念**

  > maven 工程依赖响应的jar 包、
  >
  > 第一，它会去maven 配置文件中，找到对应的配置，先去**本地仓库**。
  >
  > 第二，如果本地仓库没有，会进行**私服仓库**
  >
  > 第三，私服仓库没有，进行**远程仓库找**。
  >
  > 如此类推，只要找到 就**会将层级放入到该仓库**，最终**保存到本地仓库**，然后给**对应的Maven工程**进行依赖。 

![image](9468596DE3F34FC29E86BBF2C63E8E0E)

- b.**作用**

  > - 用于微服务接口发布和调用（引入的是Jar 包进行调用）。
  >
  > - 第三方Jar包的管理。
  
  
## 2. 基于Docker搭建Maven私服

1. 下载一个nexus3的镜像
```shell
docker pull sonatype/nexus3
```
2. 将容器内部 /var/nexus-data 挂载到主机 /root/nexus-data 目录。
```shell
docker run -d -p 8081:8081 --name nexus -v /root/nexus-data:/var/nexus-data --restart=always sonatype/nexus3
```
3. 关闭防火请
```shell
systemctl stop firewalld
```
4. 打开浏览器访问[：http://ip:8081 ](https://note.youdao.com/)
![image](69702F93C02148AFBAB1B61B740BF4EC)

  > - *Maven私服启动容器稍微比较慢，等待1分钟即可。*
  >
  > - *默认登陆账号 admin admin123*


## 3. 创建Maven私服仓库
1. 创建仓库，点击 **Create repository**, 然后选择 **maven2(hosted)** ,然后输入**仓库名称（test-release）**。
![image](35FF71FBE69A43ACBBDDEA8AC969797C)

2. 在 **version policy** 中选择这个仓库的类型，这里选择 **release** ,在 **Deployment policy** 中选择 **Allow redeploy（这个很重要）**
![image](FA22FB77476746AB9879C73405D6B57B)


## 4. 创建私服账号
1. 点击左侧菜单栏的 **Users菜单**，然后 **点击Create local user**， 我这里创建了一个用户。
![image](DBDE0B461A284A3EA8D892D12DB7FE69)

 > - *账号：Calvin*
 > - *密码：123456*
 

## 5. 设置Maven 本地settings.xml
```xml
<servers>
	<server>
        <id>Calvin</id>
        <username>Calvin</username>
        <password>123456</password>
      </server>
  </servers>
```

## 6. 创建一个Maven 工程
创建一个maven工程，并且。
```xml
<!--注意限定版本一定为RELEASE,因为上传的对应仓库的存储类型为RELEASE -->
<!--指定仓库地址 -->
<distributionManagement>
	<repository>
		<!--此名称要和.m2/settings.xml中设置的ID一致 -->
		<id>Calvin</id>
		<url>http://192.168.182.131:8081/repository/calvin-release/</url>
	</repository>
</distributionManagement>

<build>
	<plugins>
		<!--发布代码Jar插件 -->
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-deploy-plugin</artifactId>
			<version>2.7</version>
		</plugin>
		<!--发布源码插件 -->
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-source-plugin</artifactId>
			<version>2.2.1</version>
			<executions>
				<execution>
					<phase>package</phase>
					<goals>
						<goal>jar</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
```

## 7.打包到maven私服
```shell
mvn deploy
```

## 8. 测试依赖信息
```xml
<dependencies>
	<dependency>
		<groupId>com.Calvin</groupId>
		<artifactId>org.springboot2.x</artifactId>
		<version>0.0.1-RELEASE</version>
	</dependency>
</dependencies>

<repositories>
	<repository>
		<id>Calvin</id>
		<url>http://192.168.182.131:8081/repository/calvin-release/</url>
	</repository>
</repositories>
```

## 9. 如何判断文件是否发生改变 
> - *当然是用比较文件hash值的方法，文件hash又叫文件签名，文件中哪怕一个bit位被改变了，文件hash就会不同。比较常用的文件hash算法有MD5和SHA-1。*
