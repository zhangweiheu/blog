---
title: SSH权限
tags: [SSH]
categories: [Linux]
comments: true
toc: true
date: 2016-05-28 21:31:39
description: ssh权限
---
#### ssh权限  

　今天Win机器换了密钥对，所以要更新机器上的公钥，在给自己新增加的用户添加完密钥以后，Xshell登陆时发现使用新的密钥登陆不上去，提示密钥未在远程主机上注册，在google以后发现是文件夹和authorized_keys的权限问题。　

---