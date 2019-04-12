---
date: 2019-04-11 16:05:38
type: "docker"
tags: 
	- docker
	- linux
	- shell
comments: true
title: docker 简单使用
---
e# Docker 使用
* dockfile使用
docker 如何发布jar
<!--more-->
```linux
FROM openjdk:8-jdk-alpineENV SPRING_OUTPUT_ANSI_ENABLED=ALWAYS \    
JHIPSTER_SLEEP=0 \    
JAVA_OPTS=""	
# add directly the war
ADD *.jar /app.jar
#设置运行景象 和北京时间
RUN \    
echo -e "https://mirrors.ustc.edu.cn/alpine/latest-stable/main\nhttps://mirrors.ustc.edu.cn/alpine/latest-stable/community" > /etc/apk/repositories && \    
apk update && \    
apk add --no-cache openssh tzdata && \    
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \    
echo "xiaoshanyunwei" >  /etc/timezone && \    
rm -rf /var/cache/apk/*
EXPOSE 8094 5701/udp
CMD echo "The application will start in ${JHIPSTER_SLEEP}s..." && \    
sleep ${JHIPSTER_SLEEP} && \    
java ${JAVA_OPTS} -Djava.security.egd=file:/dev/./urandom -jar /app.jar


```
	
* 第二种写法

```
dockerfile 设置服务景象 
FROM openjdk
MAINTAINER lcc
ENV JAVA_HOME /use/local/java
RUN echo $JAVA_HOME
ADD client_lcc-V1.jar /root/lcc/docker/app.jar
VOLUME ["/root/lcc/docker"]
WORKDIR /root/lcc/docker
EXPOSE 8888/tcp
ENTRYPOINT ["java","-jar","/root/lcc/docker/app.jar"] 
CMD java $JAVA_HOME -Djava.security.egd=file:/dev/./urandom -jar /root/lcc/docker/app.jar
 
```

* CMD	容器启动时运行的操作。该指令只能在文件中存在一次，如果有多个，则只执行最后一条

* ENTRYPOINT 设置容器启动时执行的操作。该指令只能在文件中存在一次，如果有多个，则只执行最后一条
* EXPOSE 指定容器需要映射到宿主机器的端口.  
* VOLUME 指定挂载点(设置运行文件存放的路径)
* WORKDIR 切换目录。可以多次切换工作目录(相当于cd命令)
* ENV 指定容器运行环境
* docker build -t lcc/test .	打包当前Dockerfile并指定名称
* docker run -d（后台运行） -p（端口映射 主机端口：容器端口）--env-file=／dev.sh (读取环境变量) lcc
* -m :设置容器使用内存最大值
* rm ID	删除docker容器(先删除容器，在删除镜像)
* rmi ID	删除docker镜像
* docker logs -f lcc(容器name)|grep '关键词'
* docker ps	展示运行容器列表
* docker images	展示镜像列表
* docker save -o nginx.tar(导出名称) nginx:latest(镜像名称). docker镜像导出
* docker export -o nginx-test.tar nginx-test docker导入 支持自定义镜像名称
* docker load -i nginx.tar docker导入 全部信息导入


```
	docker rmi $(docker images -q) 删除所有镜像
	docker rm $(docker ps -a -q) 删除所有容器|stop	停止所有容器
	删除id为<None>的image
	docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
	删除指定容器
	docker rm $(docker ps -a|grep -w "name"|awk "{print $3}")
	复制容器中的配置文件到宿主机目录
	docker cp [CONTAINER ID]:/usr/local/logs /etc/docker/openapi
```
	
