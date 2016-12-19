---
title: SSH权限
tags: [SSH,命令]
categories: [Linux]
comments: true
toc: true
date: 2016-05-28 21:31:39
description: ssh权限
---
#### ssh权限

　今天Win机器换了密钥对，所以要更新机器上的公钥，在给自己新增加的用户添加完密钥以后，Xshell登陆时发现使用新的密钥登陆不上去，提示密钥未在远程主机上注册，在google以后发现是文件夹和authorized_keys的权限问题。
#### 常用的几个SSH方面的命令
1、ssh-keygen
```bash
ssh-keygen -t rsa
```
生成RSA加密协议的公私钥对，非对称加密；一般在用户目录的.ssh目录下（即：~/.ssh），其中id_rsa是密钥对的私钥，id_rsa.pub是密钥对的公钥，一般公钥是公开的。
2、cat
```bash
cat id_rsa.pub >> ~/.ssh/authorized_keys
```
这是将公钥放入用户目录的认证文件，SSH登录的时候，认证协议会先去找  ~/.ssh/authorized_keys 这个文件，看看是否认证通过。
一般来说新生成的authorized_keys和.ssh目录的权限都比较高，所以需要放开相关访问权限，也就用到下面的命令，否则会认证失败。
3、chmod
```bash
chmod 600 authorized_keys
```
设置authorized_keys文件的访问权限
```bash
chmod 700 -R .ssh
```
设置.ssh目录访问权限
600/700是简写的110000000/111000000分别是用户、用户组、其他的读、写、执行权限，1代表有，0代表无

---