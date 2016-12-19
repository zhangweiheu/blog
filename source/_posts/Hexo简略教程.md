---
title: Hexo简略教程
tags: [Hexo]
categories: [Linux]
comments: true
toc: true
mathjax: true
date: 2016-09-03 11:19:36
description: 简单小结一下使用Hexo踏过的坑
---
　　本人用过CSDN,感觉还是没有自己折腾一个静态博客有意思，所以想小结一下目前使用hexo的经验。从零建站可以参考该博文：http://www.jianshu.com/p/05289a4bc8b2

  ### Hexo
　　具体这是什么，我只想说这是一个静态博客，需要用到Node.js模块，安装以及使用简单如下：
1. 安装Node.js
　　安装教程：http://www.runoob.com/nodejs/nodejs-install-setup.html
2. 安装Hexo
　　安装教程：https://hexo.io

### theme
 　　目前博客主题有很多，看自己喜好了，主题地址：https://hexo.io/themes
   推荐两个博主使用过的主题：

#### NexT
博主就是采用这个主题，功能比较完善，简洁美观，支持几种不同的风格。
作者提供了非常完善的配置说明。
作者博客： http://notes.iissnan.com
<center>
![](http://7xo04n.com1.z0.glb.clouddn.com/next.JPG)
</center>

#### Maupassant
一款非常简洁的主题
作者博客： https://www.haomwei.com/technology/maupassant-hexo.html
<center>
![](http://7xo04n.com1.z0.glb.clouddn.com/maupassant.JPG)
</center>

### 使用技巧

- 克隆主题以后将本地修改的主题，及时推送到自己的Github仓库,防止个性化修改以后配置丢失；

- hexo的整个博客最好作为一个仓库作备份，同时也可以多机同步；

- hexo配置文件和主题配置文件记得备份（做到上面两条可以忽略本条）；

- 域名可有可无，Github托管的博客有免费域名：name.github.io ,如果有域名的话可以在仓库中添加 CNAME 指向Github托管的博客，万网提供的云解析服务，万网地址：https://wanwang.aliyun.com;
- 图片存储可以使用免费的七牛云，也可以使用收费的upyun，新浪云支持图片格式化；

　　简单就写这些，有问题可以留言交流，或者Email。
---