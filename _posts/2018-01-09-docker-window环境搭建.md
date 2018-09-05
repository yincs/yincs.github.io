---
layout: post
title: "在win10上搭建docker环境"
date: 2018-09-05 14:02:08 
description: "docker在win10上的下载安装、国内镜像设置、基本使用"
tag: docker
---


这里将介绍下docker的基本使用
     

### 下载与安装

[docker官网](https://www.docker.com/) 

[windows版本下载地址](https://store.docker.com/editions/community/docker-ce-desktop-windows) 

[百度网盘下载链接](https://pan.baidu.com/s/18QhfOUuM8nwJDvSirzRawA) 密码:cxy7

> 1. 下载后一路安装。
> 2. 完成后会提示启动开启Hyper-V，安卓开发的小伙伴注意了**开启Hyper-V后不能使用模拟器**，打开模拟器会让电脑直接崩溃。


### 初步使用

docker-hello-world，可以参考[菜鸟教程](http://www.runoob.com/docker/docker-hello-world.html)上的相关例子，很不错的教程。

### 常用命令
* docker images 查看拥有的镜像
* docker rmi xxx    删除镜像
* docker ps 查看正在运行的容器
* docker ps -a  查看所有的容器
* docker rm xxx 删除容器（支持同时删除多个）
* docker run --rm -v $PWD/data:/data -p 6060:8080 -it image /bin/bash --rm表示结果后删除该容器，
-v将主机前面目录下的data文件夹挂载到虚拟机的/data目录中，-p表示把虚拟器8080端口映射到主机6060端口（随机端口用-P）,
-it 表示允许与容器进行交互，image表示用该镜像来创建容器，/bin/bash表示创建容器后在容器里执行的命令
* docker start xxx 启动容器
* docker stop xxx 停止容器
* docker exec -it xxx bash 在xxx容器内运行bash命令，-it表示可交互
* exit 退出容器


参考资料：

- [docker官网](https://www.docker.com/) 
- [docker中文社区](http://www.docker.org.cn/) 
- [docker菜鸟教程](http://www.runoob.com/docker/docker-tutorial.html)



<br>

转载请注明：[小嵩的博客](http://baixin) » [点击阅读原文](http://baixin.io/2015/09/iOS9_Note/)