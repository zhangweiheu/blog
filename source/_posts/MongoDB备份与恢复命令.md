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
```bash
./mongodump --db DbName --o /DestinationBackupPath
```
默认路径在当前路径下：dump/DbName   

### 恢复
```bash
./mongorestore --db DbName /SourceBackupPath
```
### 异常  

---