---
title:        "linux下的alias和type"
description:  "linux下的如何巧用alias"
author:       "vhomo"
---


## 软链接及其局限性
windows下有快捷方式，双击快捷键时会以所链接文件的打开方式打开，比如说：
* 如果是一个目录的快捷方式，双击打开的是所链接的目录
* 如果是一个可编辑文件的快捷方式，双击会使用该文件的默认编辑器打开此文件
* 如果是一个可执行文件的快捷方式，双击会启动相应的程序

linux下也有类似的机制，那就是软链接，不管是快捷方式还是软链接，其本质还是对文件进行操作，只是可以链接到其它地方，并以我们希望的方式命名。
在日常工作中，我们期望有一些快捷操作，比如说我们以下命令
* 查看nginx的实时请求日志 tailf /var/log/nginx/access.log
* 查看nginx的实时错误日志 tailf /var/log/nginx/error.log
* 查看nginx的最新100条请求 tail -n 100 /var/log/nginx/access.log

我们的软链接（快捷方式）可以是针对tailf或tail程序本身，但是却无法将tailf /var/log/nginx/access.log这么一个动态命令设置为一个快捷方式，
当然真的要实现也可以编写一个shell脚本（或windows下的bat）将命令本身写入到一个脚本程序中，再对脚本程序设置软链接。

## linux下的alias
上述提到软链接需要借助一个独立的脚本，然后对脚本创建软链接（或直接将脚本所在目录置于PATH环境变量之中）才可以对一个携带附加参数执行的动态命令进行快捷操作，如果只是为了在命令行终端下的快捷操作，其实linux下还有另外一个命令alias（别名），可以更优雅简便的达到我们的目的。
还是以上述三3个对nginx日志的访问为例：
* 查看nginx的实时请求日志设置别名：alias nginxlog="tailf /var/log/nginx/access.log"
* 查看nginx的实时错误日志设置别名：alias nginxerrorlog="tailf /var/log/nginx/error.log"
* 查看nginx的最新100条请求设置别名：alias nginxlastlog="tail -n 100 /var/log/nginx/access.log"

设置为上述3个快捷访问以后，日志访问操作如下：
* 查看nginx的实时请求日志：nginxlog
* 查看nginx的实时错误日志：nginxerrorlog
* 查看nginx的最新100条请求：nginxlastlog  

## type查看命令的类型

使用type可以查看程序的别名，比如以下查看nginxlastlog:  

![example image](/resources/images/20150627_type_nginxlog.jpg "type nginxlog example image")

## linux下某些命令的默认alias
ls作为linux最基础最常用的命令之一，如下所示:

![example image](/resources/images/20150627_ls.jpg "ls example image")

上述图片中可以发现，ls的结果会根据文件的不同类型，不同后缀显示不同的颜色，这个其实是因为ls命令后面加了--color=auto
![example image](/resources/images/20150627_ls_raw.jpg "ls raw example image")
![example image](/resources/images/20150627_ls_autocolor.jpg "ls auto color example image")

在终端下使用type ls可进一步的确认：
![example image](/resources/images/20150627_type_ls.jpg "type ls example image")

可以看到我们在终端下使用的ls其实是默认携带了--color=auto参数，所以如果在linux下遇到其他的命令执行结果带颜色，就可以怀疑可能是携带了--color=auto参数，比如说grep
![example image](/resources/images/20150627_type_grep.jpg "type grep example image")

再比如说cp命令，某些linux发行版下在终端执行cp命令，即使已经加了-f参数强制覆盖已存在的文件，但执行cp -f 时还是会交互性的提示是否要覆盖，这是因为所执行的cp并不是原生的cp，而是cp -i的别名，这个选项就是在覆盖前做一个提示。


