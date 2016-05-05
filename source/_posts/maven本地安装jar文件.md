---
title: Maven本地安装jar文件
tags: [Maven]
categories: [Maven]
comments: true
toc: true
date: 2016-04-17 20:54:59
description: maven本地仓库安装jar
---

###  maven本地仓库安装jar和源文件

mvn install:install-file -Dfile="f:\alipay-sdk-java-1.5-source.jar" -DgroupId="alipay" -DartifactId="sdk-java" -Dversion="1.5" -Dpackaging=jar  //-Dclassifier=sources

---

