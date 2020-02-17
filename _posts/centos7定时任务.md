---
title: CentOS 7.5 利用 crontab 定时执行任务
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


# crond 简介
crond 是linux下用来周期性的执行某种任务或等待处理某些事件的一个守护进程，与windows下的计划任务类似，当安装完成操作系统后，默认会安装此服务 工具，并且会自动启动crond进程，crond进程每分钟会定期检查是否有要执行的任务，如果有要执行的任务，则自动执行该任务。cron服务是一个定时执行的服务，可以通过crontab 命令添加或者编辑需要定时执行的任务。


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


![图](https://ws1.sinaimg.cn/large/006wG1mNgy1gbzav0d089j30f20ciweq.jpg)

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

# 查看 Crontab 定时任务启动是否成功
1. 先手动检查定时任务脚本是否能够单独运行，并且crond启动与否
   1. systemctl status crond # 查看crond的状态
   2. systemctl start crond #启动crond服务
2. 查看crontab执行纪录
   1. 如果出现crontab定时任务不执行的情况，运行命令，脚本和crond都没有出现问题，那么就需要通过日志确定问题所在。
   2. crontab的日志位置一般在/var/log/cron,可以看用下面的命令来查看日志。   
     ` tail -f /var/log/cron `  
上面的语句会纪录是否执行了某些计划的脚本，但是具体执行是否正确以及脚本执行过程中的一些信息linux会通过邮件形式发送到给该用户。 对于root用户该邮件记录位于**/var/spool/mail/root**，通过以下命令可以查看最近的crontab执行情况。  
     ` tail -f /var/spool/mail/root `
mail邮件一般只会记录脚本执行成功与否，如果执行失败，无法给出进一步的错误信息，这时需要我们将语句执行的错误信息

重定向至文件中，这样可以很方便的查看错误信息
