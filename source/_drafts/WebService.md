---
title: Web Services
tags: [WebServices]
categories: [Java]
comments: true
toc: true
mathjax: true
date: 2016-08-13 18:24:07
description: Web Services学习
---
#### SOA简介
　　SOA(Service-Oriented Architure，面向服务架构)是目前一种比较流行的架构模型，如下图：
   ![SOA](http://7xo04n.com1.z0.glb.clouddn.com/SOA.JPG)
　　虽然里面的服务不一定是Web Services,但由于WS的优越性，WS已成为最常用的服务表现形式。SOA协议栈如下图：
  ![SOA协议栈](http://7xo04n.com1.z0.glb.clouddn.com/soa%E5%8D%8F%E8%AE%AE%E6%A0%88.JPG)
　　其中比较关键的几个名词：
- 	SOAP:简单对象访问协议
- 	WSDL:Web服务描述语言

　　SOA通用设计原则:
-   一致性：所有参与者都能理解并解析服务，整合内容统一管理。
-   服务粒度：服务本身的功能大小或者服务应包含的操作数量。
-   自治：所有服务都是可以自我管理的。
-   高内聚：服务内的功能模块都是在业务上紧密相关的，这些服务都是为了一致的主题进行组装合并的，不相关的功能不应该放到同一个服务中。
-   松耦合：服务之间要求最小依赖性，一个服务的改变不影响其他服务的变化。
-   服务调用：服务平台应该建立在平台中立的技术之上，同时提供多种可传输协议和数据格式的支持。

#### Web Services
　　Web Services是一种实现SOA最常见的技术标准，正迅速成为用于支持SOA应用的事实标准。结构模型如下图：![结构模型](http://7xo04n.com1.z0.glb.clouddn.com/WebServices.JPG)
  共包含三种角色：
-   服务提供者：发布服务，从体系结构上看是提供服务的平台
-   服务请求者：寻找服务、绑定服务，从体系结构上看是客户端应用程序
-   服务注册器（服务代理）：存储服务描述信息，服务提供者在这里发布服务，服务请求者在这里查找服务、获取绑定信息

　　Java中关于SOA的相关规范
- 	JAX-RPC规范:Java Api for XML-based RPC
- 	JAX-WS规范：Java Api for XML-based Web Services
- 	JAX-RS规范：Java Api for XML-based Representational State Transfer(REST)
- 	JAXB规范：Java Api for XML Binding

#### Apahe CXF框架
　　Apache开源的Web服务框架，主要功能集合在以下几个方面：
-   Web Services标准的支持：支持多种Web Services 标准；
-   前端模式：支持多种前端模型。
-   易用性：设计明确、编码直观、易于使用。
-   二进制和遗留协议支持：可插拔架构，故可以提供整合任何传输类型的架构容器，不仅支持XML类型的容器，也支持非XML类型的绑定，如JSON和CORBA等。

