---
title: MySQL5.6源码安装
tags: [MySQL源码安装]
categories: [数据库]
comments: true
toc: true
mathjax: true
date: 2016-09-06 07:57:41
description: 在最小化安装的CentOS7系统上编译安装MySQL
---
### CentOS7开启网络
使用vi或者vim编辑器打开网络配置模块的配置文件：/etc/sysconfig/network-scripts/ifcfg-enp\*\*\* 配置如下图：
<center>
![](http://7xo04n.com1.z0.glb.clouddn.com/network_dhcp.JPG)
</center>

将BOOTPROTO设置为动态的，即：BOOTPROTO=dhcp
将ONBOOT设置为yes即：ONBOOT=yes
保存退出：wq
重启网络命令：
```bash
service network restart
```
或者重启linux系统
```bash
reboot
```
查看网络
```bash
ip addr
```
### 1、下载源码
```bash
wget http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.23.tar.gz
```
### 2、解压
```bash
tar zxvf mysql-5.6.23.tar.gz
```
### 3、安装必要的包
```bash
sudo yum install cmake gcc-c++ ncurses-devel perl-Data-Dumper
```
### 4、进入mysql源码目录，生成makefile
```bash
sudo cmake .
```
### 5、编译
```bash
make
```
### 6、安装
```bash
sudo make install
```
mysql将会安装到/usr/local/mysql路径。
### 7、添加mysql用户和组
```bash
sudo groupadd mysql
sudo useradd -r -g mysql mysql
```
### 8、修改目录和文件权限，安装默认数据库
```bash
cd /usr/local/mysql

sudo chown -R mysql .

sudo chgrp -R mysql .

sudo scripts/mysql_install_db –user=mysql

sudo chown -R root .

sudo chown -R mysql data
```
至此，mysql就可以启动运行了。
### 9、启动mysql

CentOS7自带MariaDB的支持，/etc下默认存在my.cnf文件干扰mysql运行，需要先删掉
```bash
cd /etc
sudo rm -fr my.cnf my.cnf.d
```
然后再/etc下重建my.cnf文件，内容如下(或可以不创建，但已经要将/etc/my.cnf删除，默认使用：/usr/local/mysql/my.cnf)
```file
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html

[mysqld]

# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M

# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin

# These are commonly set, remove the # and set as required.
# basedir = …..
# datadir = /data/mysql/data
# port = …..
# server_id = …..
# socket = …..

# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M

max_connection = 10000
sql_mode = NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

#binary log
log-bin = mysql-bin
binlog_format = mixed
expire_logs_day = 30

#slow query log
slow_query_log = 1
slow_query_log_file = /var/log/mysql/slow.log
long_query_time = 3
log-queries-not-using-indexes
log-slow-admin-statements
```
现在可以启动mysql了
```bash
sudo /usr/local/mysql/bin/mysqld_safe –user=mysql &
```

CentOS7 不能使用service控制mysql服务，而源码安装的mysql也没有提供Systemd的控制脚本。

于是编辑/etc/rc.d/rc.local文件，添加mysql的开机启动命令。

/usr/local/mysql/bin/mysqld_safe –user=mysql &
然后给/etc/rc.d/rc.local添加可执行权限

sudo chmod a+x /etc/rc.d/rc.local

### 10、修改root密码(或可使用mysqladmin设置密码)
```bash
$ /usr/loca/mysql/bin/mysql -u root
use mysql;
```
```mysql
UPDATE user SET password = PASSWORD(‘test2016′) WHERE user = ‘root';
GRANT ALL PRIVILEGES ON *.* TO root@’%’ IDENTIFIED BY ‘passwd2016′;
FLUSH PRIVILEGES;
```
使用mysqladmin设置密码：(使用mysqladmin可以进入到bin目录或者设置PATH)
```mysql
$ mysqladmin -u root password 123456
```
至此，安装基本完成了，一个mysql就能用了。


设置之前，我们需要先设置PATH，要不不能直接调用mysql

修改/etc/profile文件，在文件末尾添加
```bash
$ vim /etc/profile
```
```file
PATH=/usr/local/mysql/bin:$PATH
export PATH
```
关闭文件，运行下面的命令，让配置立即生效
```bash
$ source /etc/profile
```
连接本机MySQL
```bash
$ mysql –u root –p
```
提示输入password，默认为空，按Enter即


设置选项文件，将配置文件拷贝到/etc下
```bash
$ cp support-files/my-medium.cnf /etc/mysql.cnf
```
### 10.设置开机自启动
```bash
$ cp support-files/mysql.server /etc/init.d/mysql
$ chmod +x /etc/init.d/mysql
$ chkconfig mysql on
```
通过服务来启动和关闭Mysql
```bash
$ service mysql start
$ service mysql stop
```

### 11.踩坑
1. 权限问题
```bash
sudo chown -R mysql.mysql /usr/local/mysql
sudo chmod a+x /etc/my.cnf
sudo chmod a+x -R /etc/my.cnf
```
2. SELINUX问题
默认开始SELINUX，关闭之，否则无法连接网络，
```bash
sudo vim /etc/selinux/config
```
将
```file
SELINUX=enforcing
```
改为
```file
SELINUX=disabled
```
然后重启机器

3. 更新PID问题
执行 service mysql start 提示如下错误：
```file
ERROR! MySQL server PID file could not be found!
Starting MySQL. ERROR! The server quit without updating PID file (/var/lib/mysql/localhost.localdomain.pid).
```
在日志中出现了如下错误：
```file
Can't open and lock privilege tables: Table 'mysql.user' doesn't exist
Fatal error: Can't open and lock privilege tables: Table 'mysql.user' doesn't exist
```
使用下面的命令初始化以后，问题解决：
```bash
/usr/local/mysql/scripts/mysql_install_db --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data --user=mysql
```
下面是网上总结的其他方法：
1.可能是/usr/local/mysql/data/rekfan.pid文件没有写的权限
解决方法 ：给予权限，执行
```bash
chown -R mysql:mysql /var/data
chmod -R 755 /usr/local/mysql/data
```
然后重新启动mysqld！
2.可能进程里已经存在mysql进程
解决方法：用命令
```bash
ps -ef|grep mysqld
```
查看是否有mysqld进程，如果有使用
```bash
kill -9  进程号
```
杀死，然后重新启动mysqld！
3.可能是第二次在机器上安装mysql，有残余数据影响了服务的启动。
解决方法：去mysql的数据目录/data看看，如果存在mysql-bin.index，就赶快把它删除掉吧，它就是罪魁祸首了。本人就是使用第三条方法解决的 ！
4.mysql在启动时没有指定配置文件时会使用/etc/my.cnf配置文件，请打开这个文件查看在[mysqld]节下有没有指定数据目录(datadir)。
解决方法：请在[mysqld]下设置这一行
```file
datadir = /usr/local/mysql/data
```
5.skip-federated字段问题
解决方法：检查一下/etc/my.cnf文件中有没有没被注释掉的skip-federated字段，如果有就立即注释掉吧。
6.错误日志目录不存在
解决方法：使用“chown” “chmod”命令赋予mysql所有者及权限
7.给/usr/local/mysql/data 目录权限
```bash
chmod 744 -R  /usr/local/myql/data
```
8.给/var/lock/subsys/mysql 目录权限
```bash
chmod 744 -R  /var/lock/subsys/mysql
```
9.删除安装目录下的错误日志
```file
/usr/local/mysql/data/*.err
/var/log/mysql/
```


---