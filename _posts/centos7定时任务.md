---
title: centos7定时任务
date: 2020-02-17 10:24:54
author: fuf
notebook: blog
evernote-version: 0
source: 原创
thumbnail: 
tags:
    - 默认
blogexcerpt: 
---

<!-- more -->
**在windows上有计划任务，centos7 自然也有计划任务，而且设置更为灵活，好用。centos7 上可以利用crontab 来执行计划任务， 依赖与 crond 的系统服务，这个服务是系统自带的，可以直接查看状态，启动，停止**
<!-- more -->



# 安装 crontabs 服务并且设置开机启动

``` 
yun install crontabs
systemctl enable crond # 设置为开机启动
systemctl start crond # 启动crond服务

```
# 设置用户自定义定时任务
```
vi /etc/crontab

```

打开文件可以看到

```
Example of job definition:
.---------------- minute (0 - 59)
| .------------- hour (0 - 23)
| | .---------- day of month (1 - 31)
| | | .------- month (1 - 12) OR jan,feb,mar,apr ...
| | | | .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
| | | | |
* * * * * user-name command to be executed
```
上面的命令为
分钟(0-59) 小时(0-23)日(1-31)月(1-12)星期(0-6，0表示周日) 用户名 需要执行的命令 

“*”代表取值范围内的数字,

“/”代表”每”,

“-”代表从某个数字到某个数字,

“,”分开几个离散的数字


例子：
```
    */30 * * * * root /usr/local/mycommand.sh (每天，每30分钟执行一次 mycommand命令)

    * 3 * * * root /usr/local/mycommand.sh (每天凌晨三点，执行命令脚本，PS:这里由于第一个的分钟没有设置，那么就会每天凌晨3点的每分钟都执行一次命令)

    0 3 * * * root /usr/local/mycommand.sh (这样就是每天凌晨三点整执行一次命令脚本)

    */10 11-13 * * * root /usr/local/mycommand.sh (每天11点到13点之间，每10分钟执行一次命令脚本，这一种用法也很常用)

    10-30 * * * * root /usr/local/mycommand.sh (每小时的10-30分钟，每分钟执行一次命令脚本，共执行20次)

    10,30 * * * * * root /usr/local/mycommand.sh (每小时的10,30分钟，分别执行一次命令脚本，共执行2次）
```

# 保存生效
在文件中写入命令，保存。之后加载任务，使其生效：`crontab /etc/crontab`  
查看任务 ：` crontab -l `  
列出用户的定时任务列表：` crontab -u 用户名 -l  `




