---
title: Maven本地安装jar文件
tags: [Maven]
categories: [Maven]
comments: true
toc: true
date: 2016-04-17 20:54:59
description: maven本地仓库安装jar
---

###  maven本地仓库安装jar包和源码

#### 安装依赖包
``` bash  
mvn install:install-file -Dfile=D:/spymemcached-2.10.3.jar -DgroupId=net.spy -DartifactId=spymemcached -Dversion=2.10.3 -Dpackaging=jar
```
#### 安装source类的源码
```bash
mvn install:install-file -Dfile=D:/spymemcached-2.10.3-sources.jar -DgroupId=net.spy -DartifactId=spymemcached -Dversion=2.10.3 -Dpackaging=jar -Dclassifier=sources
```

#### 安装javadoc类的源码
```bash
mvn install:install-file -Dfile=D:/spymemcached-2.10.3-javadoc.jar -DgroupId=net.spy -DartifactId=spymemcached -Dversion=2.10.3 -Dpackaging=jar -Dclassifier=javadoc
```
---

