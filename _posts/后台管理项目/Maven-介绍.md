---
title: Maven-介绍
date: 2019-03-04 00:17:45
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---

 


# Maven
Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。
## 使用Maven 好处
1. 方便管理各种库文件依赖。
2. 标准化构建流程。
3. 在持续集成中扮演重要作用。
<!-- more -->
## 整合
## Maven 仓库
由于默认的仓库地址是国外地址，会对下载速度有一定的影响，下载jar包的时候，如果没有将仓库设置为国内链接的话，可能会导致下载慢的情况，这种情况也很好解决，设置国内镜像或者国内仓库
几个国内可用的maven repository连接：
http://maven.oschina.net/content/groups/public/
http://maven.oschina.net/content/repositories/thirdparty/

阿里云的Maven仓库地址：
```
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```
## setting.xml 文件
settings.xml文件是用来设置maven参数的配置文件，并且，settings.xml是maven的全局配置文件，而pom.xml文件是所在项目的局部配置。  
settings.xml中包含类似本地仓储位置、修改远程仓储服务器、认证信息等配置。  
下面是一个settings.xml的示例文件：
```
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <!-- 本地仓库：本地存放jar包的文件夹位置 -->
    <localRepository>/xx/xx</localRepository>
    <pluginGroups>
    </pluginGroups>
    <proxies>
    </proxies>
    <servers>
    </servers>
    <!--加速镜像，也可以考虑阿里云的maven镜像 -->
    <mirrors>
        <mirror>
            <id>UK</id>
            <mirrorOf>central</mirrorOf>
            <url>http://uk.maven.org/maven2</url>
        </mirror>
        <mirror>
            <id>net-cn</id>
            <mirrorOf>central</mirrorOf>
            <url>http://maven.net.cn/content/groups/public/</url>
        </mirror>
        <mirror>
            <id>osc</id>
            <mirrorOf>central</mirrorOf>
            <url>http://maven.oschina.net/content/groups/public/</url>
        </mirror>
        <mirror>
            <id>osc_thirdparty</id>
            <mirrorOf>thirdparty</mirrorOf>
            <url>http://maven.oschina.net/content/repositories/thirdparty/</url>
        </mirror>
    </mirrors>
    <profiles>
        <profile>
            <id>osc</id>
            <activation>
                <!--当前使用的远程仓库为osc-->
                <activeByDefault>true</activeByDefault>
            </activation>
            <repositories>
                <repository>
                    <id>osc</id>
                    <!--当前使用的远程仓库地址 -->
                    <url>http://maven.oschina.net/content/groups/public/</url>
                </repository>
                <repository>
                    <id>osc_thirdparty</id>
                    <url>http://maven.oschina.net/content/repositories/thirdparty/</url>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>osc</id>
                    <url>http://maven.oschina.net/content/groups/public/</url>
                </pluginRepository>
            </pluginRepositories>
        </profile>
        <profile>
            <id>net-cn</id>
            <repositories>
                <repository>
                    <id>net-cn</id>
                    <url>http://maven.net.cn/content/groups/public/</url>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>net-cn</id>
                    <url>http://maven.net.cn/content/groups/public/</url>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
</settings>

```




