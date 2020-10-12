---
layout: ew
title: Docker搭建webgoat
date: 2020-10-12 12:35:16
tags:
---

### 虚拟机：Ubuntu18 服务器版 

<!-- more -->

### 开始搭建

1. 在虚拟机上安装docker；
2. `$ sudo docker search webgoat` 查找webgoat镜像；
3. `$ sudo docker pull webgoat/webgoat-8.0` 拉取镜像；
4. `$ sudo docker run -dt --name webgoat -p 8080:8080 --rm webgoat/webgoat-8.0` 开启镜像服务；
5. 使用浏览器访问：ip:8080/WebGoat 即可；
6. 自己创建账户登录，即可使用，全英文界面可能对和我一样的英语白痴来说不太友好；

> 网上看到一些其他的开启镜像的方法，我使用的时候一直卡着启动不了。

### 联合搭建webgoat与webwolf

1. 使用docker拉取镜像：` sudo docker pull webgoat/goatandwolf `

2. 启动镜像：`sudo docker run -d -p 8081:8080 -p 9090:9090 -e TZ=Europe/Amsterdam webgoat/goatandwolf `

   >可能会出现浏览器兼容问题