---
layout: post
title: "kafka常见问题解决方案"
date: 2018-09-08 12:00:00
description: "开发过程中遇到的kafka常见问题解决记录笔记"
tag: kafka
---
     

### 问题列表与解决方案

1. Connection to node -1 could not be established. Broker may not be available.
    
    在kafka/conf/server.properties中，  
    将listeners = PLAINTEXT://your.host.name:9092  advertised.listeners=PLAINTEXT://your.host.name:9092  
    其中的your.host.name换成本机ip即可  

<br>

2. 虚拟机可以发送和接收消息，但主机却不可以，修改  
    listeners = PLAINTEXT://0.0.0.0:9092
    advertised.listeners=PLAINTEXT://192.168.169.1:9092     host为主机ip

转载请注明：[小嵩的博客](http://changs.top) » [点击阅读原文](http://changs.top/2018/09/android-upgrade-runself/)
