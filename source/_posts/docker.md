---
title: Docker
tags: [Docker,Dockerfile]
categories: [Docker]
comments: true
toc: true
date: 2016-04-27 13:05:50
---
##  Docker学习  

###  docker安装(系统必须64位)  

- centos7:yun install docker,由于docker国内官方没有站点，比较慢，所以建议使用镜像站，安装docker

###  dockerfile构建新的镜像  
#####  基本指令  
  RUN：镜像构建时要运行的命令，不同于使用docker run启动容器；  
  CMD：容器启动时要运行的命令，docker run中的命令会覆盖CMD中的命令；eg:CMD["bash","-1"]  
  ENTRYPOINT：类似于CMD指令，但是该指令不容易在容器启动时被覆盖，而docker run中的参数会被再次传递给ENTRYPOINT；eg：ENTRYPOINT["nginx","-g","daemon off"]  
  WORKDIR：  
  ENV：  
  USER：指定该镜像以什么身份去运行；eg:USER zhangwei:administor  
  VOLUME：用来向基于镜像的容器添加卷    
  ADD：复制并解压文件  
  COPY：只是复制文件  
  ONBUILD：为镜像添加触发器，  
  docker 命令 ：docker exec -it containerID/NAME /bin/bash  

---