---
title: CUTMCTF
date: 2020-10-12 12:29:03
tags:
---

### web签到

<!-- more -->

步骤

![image-20200925165820245](https://i.loli.net/2020/09/25/Kq6CBSTMkHXfs3o.png)

打开发现需要GET传参但并未指明参数，多次尝试发现只需要传入?1即可；

![image-20200925170036066](https://i.loli.net/2020/09/25/QqloIgrzCchRbd3.png)

3. 传入发现需要post一个2，多次尝试发现并没有反应，于是使用burpsuite

![image-20200925171007578](https://i.loli.net/2020/09/25/lskFRmVnJd5C2zj.png)

在params里填好参数后发现2后面多了一个等号，然后就传参成功，并且发现php代码；

![image-20200925171241179](https://i.loli.net/2020/09/25/6VsZgIjQrnFXPvo.png)

明显是使用PHP伪协议查看源文件即可获得flag

![image-20200925171533778](https://i.loli.net/2020/09/25/JcMNArDHa834Rpq.png)

base64解码即可

![image-20200925171628695](https://i.loli.net/2020/09/25/FIXL79H1Gn5SoVf.png)

### Babysqli

使用burpsuite进行注入后发现空格被过滤了，使用/**/ 替代空格；

![image-20200925182450014](https://i.loli.net/2020/09/25/lwERh63IeMtduHF.png)

并且发现下方注释，猜测flag可能在password里面，使用SQL语句查询

![image-20200924001249509](https://i.loli.net/2020/09/24/ECaKH2o9sVJjhOP.png)

发现flag，成功


### Secret

发现图片，并且下载图片，按文本格式打开，后发现php代码

![图片代码](https://i.loli.net/2020/09/25/aRE3vTsg2JMOPUF.png)

进行代码审计，题目要求需要使用GET方式提交param1和param2,然后使用POST方式提交param1与 param2;file_get_contents($str1)是指需要str1以文件的形式写入值，is_numeric($str2)是判断str2是否为数字并且可以识别十进制和十六进制，str2需要等于2592000，sleep()函数是要程序沉睡一段时间，if(((string)$str1!==(string)$str2)&&(sha1($str1)===sha1($str2)))是指需要让str1与str2的字符串形式不同，并且经过sha1()后相同；绕过这些限制救可获得flag；

![image-20200923234150197](https://i.loli.net/2020/09/23/aVLGgDedcMPAiTk.png)

> 使用十六进制绕过sleep()函数；
>
> 使用data://text/plain;base64,U3V2aW5fd2FudHNfYV9naXJsZnJpZW5k向str2中写入内容
>
> 关于sha1()函数的绕过，网上查找到文章https://www.addon.pub/2017/10/13/CTF-sha1%E5%92%8CMD5/

### Babysqli2

按照传统试一试平常的注入语句，之后发现过滤了单引号，网上查询可使用斜杠转义前面的单引号；

![图片](https://i.loli.net/2020/09/25/EpH73LIr4gliABw.png)

绕过成功，但是只显示了登录成功信息，猜测多半是盲注；发现substr() mid() 等函数被过滤了，但是left()仍可以使用；

由于flag的开头为C 所以就尝试猜测flag在password中的位置![image-20200925190307198](https://i.loli.net/2020/09/25/AvFOQm5wRq7PjfY.png)

结果发现flag在第九行；之后便使用笨办法对flag进行逐个字母爆破，下方即为最终爆破结果，按照ascii码表转换为字母即可格式为		CUMTCTF{}，大括号内全为小写；

![image-20200925185852672](https://i.loli.net/2020/09/25/OxhNUdj5yHP9Mki.png)



### 简单文件包含

页面提示需要只支持本地请求，使用burpsuite，X-Forwarded-For：127.0.0.1 没有反应  然后尝试使用client-ip:127.0.0.1  成功；

![image-20200925191129537](https://i.loli.net/2020/09/25/1clwSiMaeBCGAE3.png)

发现使用了include_once()函数，并且使用了两次，该函数只能包含同一文件一次，继续网上查找方法；发现一个重复require_once()的函数的文章，是使用伪协议配合多级符号链接的办法进行绕过的；

```
php://filter/convert.base64-encode/resource=/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/var/www/html/flag.php
```

![image-20200925200726904](https://i.loli.net/2020/09/25/mYhFJAaHixUqONd.png)

base64解码即可

![image-20200925200827613](https://i.loli.net/2020/09/25/W32tCOrTM5U9uRh.png)