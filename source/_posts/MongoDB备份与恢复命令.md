---
title: MongoDB备份与恢复命令
tags: [MongoDB]
categories: [MongoDB]
comments: true
toc: true
date: 2016-05-05 20:19:45
description: MongoDB备份与恢复命令
---
先到mongodb/bin/目录下
### 备份  
./mongodump --db xxx(数据库名称)  --o /xxx/(备份路径)  
默认路径在当前路径下：dump/xxx   

### 恢复
./mongorestore --db xxx(数据库名称) ./xxx(数据库备份路径)

### 异常  

---