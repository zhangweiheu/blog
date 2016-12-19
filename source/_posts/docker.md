---
title: Docker
tags: [Docker,Dockerfile]
categories: [Docker]
comments: true
toc: true
date: 2016-04-27 13:05:50
description: Docker学习
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
###  Docker基本命令

$ sudo docker   # docker命令帮助
Usage: docker [OPTIONS] COMMAND [arg...]
 -H=[unix:///var/run/docker.sock]: tcp://host:port to bind/connect to or unix://path/to/socket to use

A self-sufficient runtime for linux containers.

Commands:
    attach    Attach to a running container
              # 当前shell下attach连接指定运行镜像
    build     Build an image from a Dockerfile
              # 通过Dockerfile定制镜像
    commit    Create a new image from a container's changes
              # 提交当前容器为新的镜像
    cp        Copy files/folders from the containers filesystem to the host path
              # 从容器中拷贝指定文件或者目录到宿主机中
    diff      Inspect changes on a container's filesystem
              # 查看docker容器变化
    events    Get real time events from the server
              # 从docker服务获取容器实时事件
    export    Stream the contents of a container as a tar archive
              # 导出容器的内容流作为一个tar归档文件[对应import]
    history   Show the history of an image
              # 展示一个镜像形成历史
    images    List images    # 列出系统当前镜像
    import    Create a new filesystem image from the contents of a tarball
              # 从tar包中的内容创建一个新的文件系统映像[对应export]
    info      Display system-wide information   # 显示系统相关信息
    inspect   Return low-level information on a container
              # 查看容器详细信息
    kill      Kill a running container
              # kill指定docker容器
    load    Load an image from a tar archive
 	      # 从一个tar包中加载一个镜像[对应save]   login     Register or Login to the docker registry server
              # 注册或者登陆一个docker源服务器
    logs      Fetch the logs of a container
              # 输出当前容器日志信息
    port      Lookup the public-facing port which is NAT-ed to PRIVATE_PORT
              # 查看映射端口对应的容器内部源端口
    pause     Pause all processes within a container
              # 暂停容器
    ps        List containers
              # 列出容器列表
    pull      Pull an image or a repository from the docker registry server
              # 从docker镜像源服务器拉取指定镜像或者库镜像
    push      Push an image or a repository to the docker registry server
              # 推送指定镜像或者库镜像至docker源服务器
    restart   Restart a running container
              # 重启运行的容器
    rm        Remove one or more containers
              # 移除一个或者多个容器
    rmi       Remove one or more images
              # 移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或-f强制删除]
    run       Run a command in a new container
              # 在一个新的容器中运行一个命令
    save      Save an image to a tar archive
              # 保存一个镜像为一个tar包[对应load]
    search    Search for an image in the docker index
              # 在docker index中搜索镜像
    start     Start a stopped containers                    # 启动容器
    stop      Stop a running containers                     # 停止容器
    tag       Tag an image into a repository                # 给源中镜像打标签
    top       Lookup the running processes of a container   # 查看容器中运行的进程信息
    unpause   Unpause a paused container                    # 取消暂停容器
    version   Show the docker version information           # 查看docker版本号
    wait      Block until a container stops, then print its exit code
              # 截取容器停止时的退出状态值
---