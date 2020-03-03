---
title: SpringBoot学习系列文章二
date: 2018-02-17 14:33:38
author: fuf
notebook: blog
evernote-version: 0
source: 原创
thumbnail: 
tags:
    - SpringBoot
blogexcerpt:
---


<!-- more -->
 ### 导读  
上一篇文章简述了Spring学习，搭建一个Spring Boot项目，并且编写了一个HelloWord类，运行成功，返回了一个HelloWord字符串，现在我们接着学习Spring的其他特性。
<!-- more -->


# SpringBoot


# 配置文件
使用Spring Boot开发web应用程序非常方便，只需要进行简单的配置，可以把更多的精力放在业务逻辑上。。Spring Boot项目中对于配置项的支持非常友好，支持两种类型的配置文件，分别是 properties 类型和 yml 类型。其中 yml 的配置文件描述的更加清晰，推荐使用。下边是一个yml格式的配置文件：
```
server:
  port:80

```

## 在程序中，使用@Value注解提出配置信息。 
```
@Value("$(port)")
private Integer port;
```
上面代码将配置中port的值注入到了变量port中。

## 使用@ConfigurationProperties
注解加载整个配置
例如，application.yml配置文件中存在如下配置

```
server:
  port: 8000
spring:
  application:
    name: gateway


```

使用@ConfigurationProperties
注解，使得改配置直接映射成为一个对象。在注解中，通过prefix字段指定要加载的配置

```
@Entity
@ConfigrationProperties(prefix="application")
public class application
{
    private Integer port;
    private String name
    
    ....
}
```

# 支持多套配置文件
Spring Boot中可以准备多套备用的配置文件，在主配置文件中，可以指定要使用的备用配置文件

```
application-dev.yml
application-pro.yml

```

在主配置文件 application.yaml 中，指定要使用的配置文件：
```
spring:
  prifiles:
    avtive: dev
```

配置文件与程序代码是分开的，在运行jar的是否，可以自由使用对应的配置文件
```
java -jar target/HelloWord-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev
```
