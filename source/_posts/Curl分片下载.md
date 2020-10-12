---
title: Curl分片下载
date: 2020-10-12 12:37:58
tags:

---

## 2020 年 天翼杯签到题

#### 题目描述：赛方给出了一个6G的大文件让我们下载，但是速度只有几k，明显不可能靠下载完成获取flag

<!-- more -->

##### 当时看见这签到提的时候完全不会，毫无头绪，比赛完才看见大佬说需要使用curl分片下载

### curl分片下载前提

服务器需要支持  HTTP Range Request，可以用curl查看请求头，例如：
` curl -I http://mirrors.ustc.edu.cn/debian-cd/amd64/ios-cd/debian-mac-9.3.0-amd64-netinst.iso`
若返回结果内包含：Accept-Ranges:bytes ，则说明这个服务器是支持 HTTP Range Request的；
若结果不包含，则可能不支持；
若结果包含Accept-Ranges:none 则表示不支持；

### 下载

` curl --range 0-5000000000 -o  part1 <url>` --range 指定下载的某一片段；此处指定的片段大小约为50G左右；

### 合并拼接

` cat part1 part2 > outputfile` 

>此处应该注意片段文件的顺序
>拼接的方法有cat和dd，cat比较简单，容易使用；