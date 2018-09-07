---
layout: post
title: "用docker搭建vue-cli开发环境"
date: 2018-09-07 17:20:00 
description: "编写docker Dockerfile搭建vue-cli的docker镜像"
tag: docker vue
---
     

### 什么是docker镜像？

是一个已经搭建好的环境，其中的Dockerfile记录了怎么一步一步搭建这个环境的。
有了这个环境，就可以快速进行开发或者部署了


### 编写Dockerfile文件
 1. 不多说，创建Dockerfile文件
 2. 编写Dockerfile的内容
```
#指定node的镜像源
FROM node:slim

MAINTAINER cherics <cherics@163.com>
#安装淘宝镜像的cnpm
RUN npm install -g cnpm --registry=https://registry.npm.taobao.org
#安装vue-cli
RUN cnpm install -g vue-cli 
 
RUN mkdir /code
COPY . /code
 
WORKDIR /code
#对外开放8080端口
EXPOSE 8080
```

### 创建vue-cli镜像
1. 执行 docker build -t cnpm/vue-cli .  
其中cnpm/vue-cli表示镜像名，. 表示Dockerfile所在的目录

2. 执行成功后会打印
```
......
Step 6/8 : COPY . /code
 ---> caf7e52845e6
Step 7/8 : WORKDIR /code
 ---> Running in 02da2ad519a6
Removing intermediate container 02da2ad519a6
 ---> 5093efd50fc6
Step 8/8 : EXPOSE 8080
 ---> Running in 6e5726a46242
Removing intermediate container 6e5726a46242
 ---> f25c65a00f71
Successfully built f25c65a00f71
Successfully tagged cnpm/vue-cli:latest
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
```

### 创建vue-cli项目
这时执行 docker images 后可以查看有

>REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
>
>cnpm/vue-cli               latest              f25c65a00f71        4 minutes ago       251MB
<br>

运行容器 docker run --rm -it -v D:\code:/code -p 8080:8080 cnpm/vue-cli bash

如果提示"Drive has not been shared."点击桌面右下角的docker图标进入到设置，在Shard Drives将D盘共享一下

执行 **vue init webpack vue-demo** 创建vue项目

填写完项目介绍后等vue执行完，就可以在D:\code目录下看到新创建的项目，在pc上用ide工具开发即可

预览效果 **cnpm run dev**，这时在电脑打开http://localhost:8080无法访问，
因为vue启动时默认是只允许本机访问，在pc中修改vue项目配置 D:\code\vue-demo\config\index.js 将 dev.host 从localhost改成0.0.0.0 
再重新 cnpm run dev 。

如果提示“Error: ENOENT: no such file or directory, uv_cwd”，这是表示当前路径丢失，重新执行下 **“cd .”** 。



转载请注明：[小嵩的博客](http://changs.top) » [点击阅读原文](http://changs.top/2018/09/docker_started/)
