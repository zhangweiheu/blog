---
title: Docker修改系统时区
tags: [Docker,TimeZone]
categories: [Docker]
comments: true
toc: true
date: 2016-05-08 14:13:51
description: Linux修改系统时区
---
### Ubantu修改系统时区
直接修改/etc/timezone
```shell
echo "Asia/shanghai" > /etc/timezone
```

### Centos修改系统时区
```shell
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

---