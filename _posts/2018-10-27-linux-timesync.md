---
layout: post
title: "linux服务器时间同步"
date: 2018-10-27 12:00:00
description: "linux服务器时间同步"
tag: kafka
---
     

选择局域网中的一台机器作为ntp服务器，在这台机器上需要安装并启动ntpd

其他机器上要关闭ntpd，安装ntpdate
 
service ntpd stop

修改ntp服务器上的/etc/ntp.conf，加上以下的配置：

server 127.127.1.0
fudge 127.127.1.0 stratum 10

修改ntp服务器上的/etc/ntp.conf，加上以下的配置：

server 127.127.1.0
fudge 127.127.1.0 stratum 10

后面哪个数字在0-15之间都可以，这样就将这台机器的本地时间作为ntp服务提供给客户端

重启ntpd：

service ntpd restart

等五六分钟，让ntpd完成自身的时间同步，这期间可以用：

watch ntpq -p

查看状态，第6列达到17时就可以了。等待的时间是第5列poll的秒数乘以5。

然后其他的机器上执行：

ntpdate xxxx

xxxx是ntp服务器的ip地址或者主机名

转载请注明：[小嵩的博客](http://changs.top) » [点击阅读原文](http://changs.top/2018/09/android-upgrade-runself/)
