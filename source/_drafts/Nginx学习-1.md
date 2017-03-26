---
title: Nginx学习-1
tags: [Linux,服务器,Nginx]
categories: [Linux]
comments: true
toc: true
mathjax: true
date: 2017-03-02 23:09:48
description: Nginx学习第一篇，主要讲解Nginx的优势，和其他服务器进行比较
---
　　Web Server主要用来处理HTTP请求，监听特定的端口，提供相应的服务。目前主流的Web Server简单的依据其核心特性可以分成两大类：HTTP Server、Application Server

- HTTP Server
Apache、IIS、Lighttpd、Nginx
主要提供静态HTML页面的处理.

- Application Server
Tomcat、JBoss、WebLogic、GlassFish
更准确的说应该是Web Container.

- - -
#### 一、服务器简单介绍

** Apache HTTP Server **
　　以“进程”为基础的结构，在多处理器环境下性能下降，扩容时通常使用增加服务器而不是增加处理器的方式。

**  IIS Server **
　　微软的产品，具备Web服务器特性的同时还包含Gopher Server、FTP Server的，并且可以用于HTTP Server、FTP Server、NNTP Server、SMTP Server，配合Windows Server整体效果不错，但由于是付费软件，成本高的问题不容忽视。

**  Lighttpd Server **
　　开源轻量级，低内存开销、CPU占用率，支持压缩、URL重写等，但是由于功能存在不足，代理功能不完善等缺点，被Nginx后来居上。

** Apache Tomcat 服务器 **
　　免费开源的，Sun推荐的Servlet、JSP容器，轻量级，也可以作为HTTP Server，目前通常配合Nginx使用，增强承载能力。


- - -


#### 二、Nginx
　　以功能丰富著称，既可作为HTTP服务器，也可作为反向代理或者邮件服务器；能快速响应HTML请求；支持FastCGI、SSL、Virtual Host、URL Rewrite、HTTP Basic Auth、Gzip等大量功能，并支持第三方功能模块扩展。
- 提供基本HTTP服务，可作为HTTP代理服务器和反向代理服务器，支持通过缓存加速访问，可以完成简单的负载均衡和容错，支持包过滤、SSL等；
- 高级HTTP服务，可自定义配置，支持虚拟主机、URL重写、网络监控、流媒体传输；
- 可作为邮件代理服务器

　　在实际的生产应用中，主要使用Nginx的负载均衡功能，因此下文将主要介绍Nginx的反向代理、负载均衡，负载均衡是建立在反向代理的基础上。

##### 2.1 安装

Windows安装直接下载解压即可运行，以下将分析CentOS7下的编译安装过程：
2.1.1、安装依赖
```bash
$ sudo yum -y install gcc gcc-c++ autoconf automake
$ sudo yum -y install zlib zlib-devel openssl openssl-devel pcre-devel
```
2.1.2、下载解压安装nginx
```bash
$ cd /usr/local
$ sudo wget http://nginx.org/download/nginx-1.10.3.tar.gz
$ sudo tar -zxvf ./nginx-1.10.3.tar.gz
$ cd ./nginx-1.10.3
$ make && make install
```
2.1.3、配置启动脚本，添加开机启动服务
```bash
$ sudo vim /etc/init.d/nginx
```
nginx官方脚本，
nginx=”/usr/local/nginx/sbin/nginx” 修改成nginx执行程序的路径。
NGINX_CONF_FILE=”/usr/local/nginx/conf/nginx.conf” 修改成配置文件的路径。
内容如下：
```shell
#!/bin/sh
#
# nginx - this script starts and stops the nginx daemon
#
# chkconfig:   - 85 15
# description:  NGINX is an HTTP(S) server, HTTP(S) reverse \
#               proxy and IMAP/POP3 proxy server
# processname: nginx
# config:      /etc/nginx/nginx.conf
# config:      /etc/sysconfig/nginx
# pidfile:     /var/run/nginx.pid
# Source function library.
. /etc/rc.d/init.d/functions
# Source networking configuration.
. /etc/sysconfig/network
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
nginx="/usr/local/nginx/sbin/nginx"
prog=$(basename $nginx)
NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
lockfile=/var/lock/subsys/nginx
make_dirs() {
   # make required directories
   user=`$nginx -V 2>&1 | grep "configure arguments:" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`
   if [ -z "`grep $user /etc/passwd`" ]; then
       useradd -M -s /bin/nologin $user
   fi
   options=`$nginx -V 2>&1 | grep 'configure arguments:'`
   for opt in $options; do
       if [ `echo $opt | grep '.*-temp-path'` ]; then
           value=`echo $opt | cut -d "=" -f 2`
           if [ ! -d "$value" ]; then
               # echo "creating" $value
               mkdir -p $value && chown -R $user $value
           fi
       fi
   done
}
start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    make_dirs
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}
stop() {
    echo -n $"Stopping $prog: "
    killproc $prog -QUIT
    retval=$?
    echo
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}
restart() {
    configtest || return $?
    stop
    sleep 1
    start
}
reload() {
    configtest || return $?
    echo -n $"Reloading $prog: "
    killproc $nginx -HUP
    RETVAL=$?
    echo
}
force_reload() {
    restart
}
configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}
rh_status() {
    status $prog
}
rh_status_q() {
    rh_status >/dev/null 2>&1
}
case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
        exit 2
esac
```
设置访问权限、服务、开机启动：
```bash
$ sudo chmod a+x /etc/init.d/nginx
$ sudo chkconfig --add /etc/init.d/nginx
$ sudo service nginx start
$ sudo service nginx stop
$ sudo service nginx reload
$ sudo chkconfig nginx on
```
以上是nginx编译安装过程，当然还有更方便的apt-get install 、yum install等便捷方式。

##### 2.2 通用命令
2.2.1、检查配置文件
```bash
$ sudo /usr/local/nginx/sbin/nginx -t
```
2.2.2、指定配置文件
```bash
$ sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```
2.2.3、启动
```bash
$ sudo /usr/local/nginx/sbin/nginx
```
2.2.4、服务配置文件重新载入
```bash
$ sudo /usr/local/nginx/sbin/nginx -s reload
```
##### 2.3 配置文件
　　对于nginx来说，主目录的配置文件最好不要直接添加功能，而是通过 include 的方式引入其他配置文件，这样做的好处是不至于因为功能性配置文件的问题，导致nginx无法启动，这样最多是功能性服务不可用，nginx正常运行。
  nginx.conf：

```nginx.conf
user  www www;                                                             #全局块
worker_processes  1;
pid        /usr/local/nginx/logs/nginx.pid;

events {                                                                   #events块
	use epoll;                                                       #事件驱动模型的选择
    worker_connections  1024;                      #单个work process同时开启的最大连接数
	 multi_accept on;                       #每个work process是否同时接收多个新到达的请求
}

http {                                                                      #http块
    include       mime.types;                                               #资源类型
    default_type  application/octet-stream;            #默认
    server_names_hash_bucket_size 128;
    client_header_buffer_size 64k;
    large_client_header_buffers 8 64k;
    client_max_body_size 50m;
    sendfile   on;
	tcp_nopush on;
	keepalive_timeout 60;
	tcp_nodelay on;
	fastcgi_connect_timeout 600;

	server {                                                              #server块
        listen       80;                                                  #监听端口
        server_name  localhost;

        location / {
            root   html;                                                   #location块
            index  index.html index.htm;
			}

		error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
			}
}
```
** 全局块 **
　　默认配置文件的开始到events块之前的一部分内容，主要设置影响服务器整体运行的配置指令，因此作用域是Nginx全局

** events块 **
　　主要设置与影响nginx服务器与用户的网络连接的指令，主要设置包括
 -  是否开启对多work process 下的网络连接进行序列化
 -  是否允许同时接收多个网络连接
 -  选取哪种事件驱动模型处理连接请求
 -  每个work process同时支持的最大连接数

** http块 **
　　服务器配置的重要部分，代理、缓存、日志定义等绝大多数功能和第三方模块的配置都可以放在这个模块中，只有一个。http全局块常用可配置指令：
- 文件引入
- MIME-Type定义
- 日志自定义
- 是否使用sendfile文件传输
- 连接超时时间
- 单连接请求数上限

** server块 **
　　类似于虚拟主机的概念，可理解为功能的划分,可配置多个server；
** location块 **
　　server块的一个指令，一个server可包含多个location，主要作用是基于nginx服务器收到的请求字符串（例如：server name/uri-string）对除虚拟主机之外的字符串进行匹配，对特定的请求进行处理。主要实现以下功能，许多第三方模块也在该块中提供功能。
- 地址定向
- 数据缓存
- 应答控制





















---